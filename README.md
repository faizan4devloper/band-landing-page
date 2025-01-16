def lambda_handler(event, context):
    print(f"Received event: {json.dumps(event)}")
    http_method = event['requestContext']['http']['method']
    domain_name = event['requestContext']['domainName']
    stage = event['requestContext']['stage']
    http_endpoint = f"https://{domain_name}/{stage}/"
    websocket_url = os.environ['WEBSOCKET_URL']
    bucket_name = os.environ['IMAGE_BUCKET_SUBMITTED_BY_UI']

    if http_method == 'OPTIONS':  # Handle preflight request
        return {
            'statusCode': 200,
            'headers': {
                'Access-Control-Allow-Origin': '*',
                'Access-Control-Allow-Methods': 'OPTIONS, POST, GET',
                'Access-Control-Allow-Headers': 'Content-Type',
            },
        }

    if http_method == 'POST':
        try:
            body = json.loads(event['body'])
            image_data = body.get('image')
            if not image_data:
                return {
                    'statusCode': 400,
                    'headers': {'Access-Control-Allow-Origin': '*'},
                    'body': json.dumps({'message': 'Image data not provided'})
                }

            image_data = base64.b64decode(image_data)
            image_key = f"images/{context.aws_request_id}.png"
            s3.put_object(Bucket=bucket_name, Key=image_key, Body=image_data, ContentType='image/png')

            image_url = f"{bucket_name}.s3.amazonaws.com/{image_key}"
            return {
                'statusCode': 200,
                'headers': {'Content-Type': 'application/json', 'Access-Control-Allow-Origin': '*'},
                'body': json.dumps({'imageUrl': image_url})
            }
        except ClientError as e:
            print(e)
            return {
                'statusCode': 500,
                'headers': {'Access-Control-Allow-Origin': '*'},
                'body': json.dumps({'message': 'Error uploading image to S3'})
            }
        except Exception as e:
            print(e)
            return {
                'statusCode': 500,
                'headers': {'Access-Control-Allow-Origin': '*'},
                'body': json.dumps({'message': 'Internal server error'})
            }
    else:
        return {
            'statusCode': 405,
            'headers': {'Access-Control-Allow-Origin': '*'},
            'body': json.dumps({'message': 'Method not allowed'})
        }



const response = await fetch(httpEndpoint, {
  method: 'POST',
  mode: 'cors', // Ensure CORS mode is enabled
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    image: base64,
    filename: file.name,
    fileType: file.type,
  }),
});
