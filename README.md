import os
import json
import base64
import boto3
from botocore.exceptions import ClientError

s3 = boto3.client('s3')

def lambda_handler(event, context):
    print(f"Received event: {json.dumps(event)}")
    http_method = event['requestContext']['http']['method']
    domain_name = event['requestContext']['domainName']
    stage = event['requestContext']['stage']
    http_endpoint = f"https://{domain_name}/{stage}/"
    websocket_url = os.environ['WEBSOCKET_URL']
    bucket_name = os.environ['IMAGE_BUCKET_SUBMITTED_BY_UI']

    if http_method == 'GET':
        html_content = f"""
        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>HCLTech AI Insurance Agent - Demo</title>
            <style>
                /* Styling omitted for brevity */
            </style>
        </head>
        <body>
            <div id="chat-container">
                <div id="chat-header">HCLTech Insurance Agent - Demo</div>
                <div id="chat-messages"></div>
                <div id="trace-output" style="border: 1px solid #ccc; padding: 10px; margin-top: 10px; display: none;">
                    <h3>Agent Trace:</h3>
                    <ul id="trace-steps"></ul>
                </div>
                <input type="text" id="user-input" placeholder="Type your message...">
                <button id="send-button">Send</button>
                <input type="file" id="image-upload" accept="image/*" style="display: none;">
                <button id="upload-button">Upload Image</button>
                <button id="trace-button">Enable Agent Trace</button>
            </div>
            <script>
                document.addEventListener('DOMContentLoaded', function() {{
                    const ws = new WebSocket('{websocket_url}');
                    let sessionId = null;

                    ws.onopen = function() {{
                        console.log('Connected to WebSocket');
                    }};
                    ws.onclose = function() {{
                        console.log('Disconnected from WebSocket');
                    }};
                    ws.onmessage = function(event) {{
                        const message = JSON.parse(event.data);
                        if (message.type === 'trace_data') {{
                            const traceOutput = document.getElementById('trace-output');
                            const traceSteps = document.getElementById('trace-steps');
                            traceOutput.style.display = 'block';
                            traceSteps.innerHTML = ''; // Clear previous steps
                            message.data.steps.forEach(step => {{
                                traceSteps.innerHTML += `<li>${step}</li>`;
                            }});
                        }}
                    }};

                    const traceButton = document.getElementById('trace-button');
                    traceButton.addEventListener('click', function() {{
                        const payload = {{ action: 'enable_trace' }};
                        ws.send(JSON.stringify(payload));
                    }});
                }});
            </script>
        </body>
        </html>
        """
        return {
            'statusCode': 200,
            'headers': {'Content-Type': 'text/html'},
            'body': html_content
        }
    elif http_method == 'POST':
        try:
            body = json.loads(event['body'])
            image_data = body.get('image')
            if not image_data:
                return {
                    'statusCode': 400,
                    'body': json.dumps({'message': 'Image data not provided'})
                }
            image_data = base64.b64decode(image_data)
            image_key = f"images/{context.aws_request_id}.png"
            s3.put_object(Bucket=bucket_name, Key=image_key, Body=image_data, ContentType='image/png')
            image_url = f"{image_key}"
            return {
                'statusCode': 200,
                'headers': {'Content-Type': 'application/json', 'Access-Control-Allow-Origin': '*'},
                'body': json.dumps({'imageUrl': image_url})
            }
        except ClientError as e:
            print(e)
            return {
                'statusCode': 500,
                'headers': {'Content-Type': 'application/json', 'Access-Control-Allow-Origin': '*'},
                'body': json.dumps({'message': 'Error uploading image to S3'})
            }
        except Exception as e:
            print(e)
            return {
                'statusCode': 500,
                'body': json.dumps({'message': 'Internal server error'})
            }
    elif http_method == 'TRACE':  # Handle trace request
        trace_steps = [
            "Step 1: User clicked the button.",
            "Step 2: API trace request received.",
            "Step 3: Processing trace data.",
            "Step 4: Trace data sent to WebSocket."
        ]
        return {
            'statusCode': 200,
            'headers': {'Content-Type': 'application/json'},
            'body': json.dumps({'type': 'trace_data', 'data': {'steps': trace_steps}})
        }
    else:
        return {
            'statusCode': 405,
            'body': json.dumps({'message': 'Method not allowed'})
        }
