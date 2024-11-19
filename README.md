import json
import boto3
import uuid
import os
import random

# Configure S3 client
s3 = boto3.client('s3')
bucket_name = "aimlusecasesv1"
headers = {
            'Access-Control-Allow-Origin': '*',  # Replace with your client's origin
            'Access-Control-Allow-Headers': '*',
            'Access-Control-Allow-Methods': '*',  # Adjust based on the allowed methods
        }
def generate_ci_number(length):
    # Ensure the length is at least 3 (2 for "CI" and at least 1 digit)
    if length < 3:
        raise ValueError("Length must be at least 3")

# Generate random digits for the remaining length
    digits = ''.join([str(random.randint(0, 9)) for _ in range(length - 2)])

# Combine "CI" with the random digits
    ci_number = "CI" + digits

    return ci_number

def lambda_handler(event, context):
    
    print("EVENT",event)
    print("KEYS",event.keys())
    # print("EXTENSION", event['queryStringParameters']['extension'])
    print("VALUES",event.values())
    file_namewext = event['body']
    file_namewext=json.loads(file_namewext)#['payload']['filename']#via api
    #file_namewext = event['payload']['filename'] # local testing
    file_namewext=file_namewext['payload']['filename']
    dot_index = file_namewext.rfind('.')
    if dot_index == -1:  # No dot found, assume no extension
        file_name = file_namewext
        file_extension = ''
    else:
        file_name = file_namewext[:dot_index]
        file_extension = file_namewext[dot_index+1:]
    claimid = generate_ci_number(8)
    print("FILENAME",file_name)
    print("CLAIMID",claimid)
    content_type = f'application/{file_extension}'  # Set content type based on file extension
    # subfolder = f'iassureclaim/claimforms/{claimid}/'
    subfolder = f'singlifepoc/productsheet/{claimid}/'
    
    # key = f'{uuid.uuid4()}.{file_extension}'  # Generate a unique key with specified extension
    key = f'{subfolder}{file_name}'
    # Generate pre-signed URL
    presigned_url = s3.generate_presigned_url(
        ClientMethod='put_object',
        Params={
            'Bucket': bucket_name, 
            'Key': key,
            'ContentType': content_type,
        },
        ExpiresIn=3600,    # The expiration time of the URL (in seconds), here it's set to 1 hour
        HttpMethod='PUT'   # Only allow PUT requests on the url
    )

    print("PRESIGNEDURL",presigned_url)
    print("KEY",key)
    return {
    'statusCode': 200,
    'headers': headers,
    'body': json.dumps({
            'presignedUrl': presigned_url,
            'key': key
        })
    }
