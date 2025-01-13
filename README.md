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
        # HTML content for the index.html page
        html_content = f"""
        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>HCLTech AI Insurance Agent - Demo</title>
            <style>
                body {{ font-family: Helvetica, sans-serif; margin: 0; padding: 0; display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100vh; background-color: #f5f5f5; }}
                #chat-container {{ width: 500px; border: 1px solid #ccc; padding: 10px; background-color: #fff; box-shadow: 0 0 10px rgba(0, 0, 0, 0.1); border-radius: 10px; position: relative; }}
                #chat-messages {{ height: 500px; overflow-y: scroll; border-bottom: 1px solid #ccc; margin-bottom: 10px; }}
                #chat-header {{ height: 5%; border-top-left-radius: 15px; border-top-right-radius: 15px; display: flex; color: white; font-size: 20px; padding: 0.5em 0; box-shadow: 0 0 3px rgba(0, 0, 0, 0.2); justify-content: center; align-items: center; background-color: #785CE5; }}
                .user-message, .bot-message {{ text-align: left; margin: 10px; padding: 10px; background-color: #CCCCFF; border-radius: 10px; }}
                .bot-message {{ text-align: left; background-color: #eee; }}
                .typing-indicator {{ text-align: left; margin: 10px; padding: 10px; background-color: #eee; border-radius: 10px; display: flex; align-items: center; }}
                .dot {{ height: 8px; width: 8px; margin: 0 2px; background-color: #bbb; border-radius: 50%; display: inline-block; animation: blink 1.4s infinite both; }}
                .dot:nth-child(1) {{ animation-delay: 0s; }}
                .dot:nth-child(2) {{ animation-delay: 0.2s; }}
                .dot:nth-child(3) {{ animation-delay: 0.4s; }}
                @keyframes blink {{
                    0% {{ opacity: 0.2; }}
                    20% {{ opacity: 1; }}
                    100% {{ opacity: 0.2; }}
                }}
                #user-input, #send-button, #upload-button, #trace-button {{ width: calc(100% - 22px); padding: 10px; margin-bottom: 10px; border: 1px solid #ccc; border-radius: 5px; }}
                #send-button, #upload-button, #trace-button {{ background-color: #785CE5; color: white; border: none; cursor: pointer; }}
                #send-button:hover, #upload-button:hover, #trace-button:hover {{ background-color: #0056b3; }}
                #trace-steps-container {{ position: absolute; top: 10px; right: -300px; width: 280px; height: 480px; overflow-y: scroll; border: 1px solid #ccc; padding: 10px; background-color: #fff; box-shadow: 0 0 10px rgba(0, 0, 0, 0.1); border-radius: 10px; display: none; }}
                .trace-step {{ margin-bottom: 10px; padding: 10px; background-color: #f5f5f5; border-radius: 5px; }}
            </style>
        </head>
        <body>
            <div id="chat-container">
                <div id="chat-header">HCLTech Insurance Agent - Demo</div>
                <div id="chat-messages"></div>
                <input type="text" id="user-input" placeholder="Type your message...">
                <button id="send-button">Send</button>
                <input type="file" id="image-upload" accept="image/*" style="display: none;">
                <button id="upload-button">Upload Image</button>
                <button id="trace-button">Enable Agent Trace</button>
                <div id="trace-steps-container"></div>
            </div>
            <script>
                document.addEventListener('DOMContentLoaded', function() {
                    const ws = new WebSocket('{websocket_url}');
                    let sessionId = null;

                    ws.onopen = function() { console.log('Connected to WebSocket'); };
                    ws.onclose = function() { console.log('Disconnected from WebSocket'); };
                    ws.onmessage = function(event) {
                        const message = JSON.parse(event.data);
                        const chatMessages = document.getElementById('chat-messages');
                        const typingIndicator = document.querySelector('.typing-indicator');
                        if (typingIndicator && message.type === 'bot_response') {
                            chatMessages.removeChild(typingIndicator);
                        }
                        if (message.type === 'bot_response') {
                            chatMessages.innerHTML += '<div class="bot-message">' + message.data + '</div>';
                            if (message.sessionId) {
                                sessionId = message.sessionId;
                            }
                        }
                        if (message.type === 'trace_data') {
                            const traceStepsContainer = document.getElementById('trace-steps-container');
                            traceStepsContainer.style.display = 'block';
                            traceStepsContainer.innerHTML = '';
                            message.data.forEach(step => {
                                traceStepsContainer.innerHTML += '<div class="trace-step">' + step + '</div>';
                            });
                        }
                    };

                    const sendButton = document.getElementById('send-button');
                    const userInput = document.getElementById('user-input');
                    const chatMessages = document.getElementById('chat-messages');
                    const uploadButton = document.getElementById('upload-button');
                    const imageUpload = document.getElementById('image-upload');
                    const traceButton = document.getElementById('trace-button');

                    function showTypingIndicator() {
                        const typingIndicator = document.createElement('div');
                        typingIndicator.className = 'typing-indicator';
                        typingIndicator.innerHTML = '<span class="dot"></span><span class="dot"></span><span class="dot"></span>';
                        chatMessages.appendChild(typingIndicator);
                        chatMessages.scrollTop = chatMessages.scrollHeight;
                    }

                    sendButton.addEventListener('click', function() {
                        const message = userInput.value;
                        if (!message.trim()) return;
                        chatMessages.innerHTML += '<div class="user-message">' + message + '</div>';
                        userInput.value = '';
                        showTypingIndicator();
                        const payload = { action: 'sendmessage', inputText: message };
                        if (sessionId) {
                            payload.sessionId = sessionId;
                        }
                        ws.send(JSON.stringify(payload));
                    });

                    userInput.addEventListener('keypress', function(event) {
                        if (event.key === 'Enter') {
                            const message = userInput.value;
                            if (!message.trim()) return;
                            chatMessages.innerHTML += '<div class="user-message">' + message + '</div>';
                            userInput.value = '';
                            showTypingIndicator();
                            const payload = { action: 'sendmessage', inputText: message };
                            if (sessionId) {
                                payload.sessionId = sessionId;
                            }
                            ws.send(JSON.stringify(payload));
                        }
                    });

                    uploadButton.addEventListener('click', function() {
                        imageUpload.click();
                    });

                    imageUpload.addEventListener('change', function(event) {
                        const file = event.target.files[0];
                        if (!file) return;
                        const reader = new FileReader();
                        reader.onload = function(e) {
                            const base64 = e.target.result.split(',')[1];
                            const response = fetch('{http_endpoint}', {
                                method: 'POST',
                                headers: { 'Content-Type': 'application/json' },
                                body: JSON.stringify({ image: base64 })
                            });
                            response.then((response) => response.json()).then((data) => {
                                const imageUrl = data.imageUrl;
                                sendImageMessage(imageUrl);
                            });
                        };
                        reader.readAsDataURL(file);
                    });

                    function sendImageMessage(imageUrl) {
                        chatMessages.innerHTML += '<div class="user-message">Image has been uploaded</div>';
                        showTypingIndicator();
                        const payload = { action: 'sendmessage', inputText: imageUrl };
                        if (sessionId) {
                            payload.sessionId = sessionId;
                        }
                        ws.send(JSON.stringify(payload));
                    };

                    traceButton.addEventListener('click', function() {
                        fetch('https://tracesData', {
                            method: 'GET',
                            headers: { 'Accept': 'application/json' },
                        })
                        .then(response => response.json())
                        .then(data => {
                            ws.send(JSON.stringify({ type: 'trace_data', data: data.traces }));
                        })
                        .catch(error => console.error('Error:', error));
                    });
                });
            </script>
        </body>
        </html>
        """
        return {
            'statusCode': 200,
            'headers': {
                'Content-Type': 'text/html',
            },
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
                'headers': {
                    'Content-Type': 'application/json',
                    'Access-Control-Allow-Origin': '*'
                },
                'body': json.dumps({'imageUrl': image_url})
            }

        except ClientError as e:
            print(e)
            return {
                'statusCode': 500,
                'headers': {
                    'Content-Type': 'application/json',
                    'Access-Control-Allow-Origin': '*'
                },
                'body': json.dumps({'message': 'Error uploading image to S3'})
            }
        except Exception as e:
            print(e)
            return {
                'statusCode': 500,
                'body': json.dumps({'message': 'Internal server error'})
            }

    else:
        return {
            'statusCode': 405,
            'body': json.dumps({'message': 'Method not allowed'})
        }








traceButton.addEventListener('click', function() {
    fetch('https:tracesData', {
        method: 'GET',
        headers: {'Accept': 'application/json'},
    })
    .then(response => response.json())
    .then(data => {
        const traceStepsContainer = document.getElementById('trace-steps');
        traceStepsContainer.innerHTML = ''; // Clear existing trace steps

        data.traces.forEach(trace => {
            const traceStep = document.createElement('div');
            traceStep.className = 'trace-step';
            traceStep.textContent = `Step: ${trace.step}, Description: ${trace.description}`;
            traceStepsContainer.appendChild(traceStep);
        });
    })
    .catch(error => console.error('Error:', error));
});



#chat-content {
    display: flex;
}

#chat-messages {
    width: 70%;
    height: 500px;
    overflow-y: scroll;
    border-right: 1px solid #ccc;
    margin-right: 10px;
}

#trace-steps {
    width: 30%;
    height: 500px;
    overflow-y: scroll;
    background-color: #f9f9f9;
    padding: 10px;
    border-radius: 10px;
}

.trace-step {
    margin-bottom: 10px;
    padding: 10px;
    background-color: #e0e0e0;
    border-radius: 5px;
}
