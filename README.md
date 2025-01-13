.chat-container {
  width: 500px;
  margin: auto;
  padding: 20px;
  background: #fff;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}
.chat-header {
  font-size: 20px;
  text-align: center;
  background: #785ce5;
  color: #fff;
  padding: 10px;
  border-radius: 10px 10px 0 0;
}
.chat-messages {
  height: 300px;
  overflow-y: auto;
  border: 1px solid #ddd;
  margin-bottom: 10px;
  padding: 10px;
}
.user-message {
  text-align: right;
  background: #ccccff;
  padding: 5px 10px;
  border-radius: 10px;
  margin-bottom: 5px;
}
.bot-message {
  text-align: left;
  background: #eee;
  padding: 5px 10px;
  border-radius: 10px;
  margin-bottom: 5px;
}





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
        # Return the HTML content
        return {
            'statusCode': 200,
            'headers': {'Content-Type': 'text/html'},
            'body': "Frontend is separate. Use React app."
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
                'headers': {'Content-Type': 'application/json', 'Access-Control-Allow-Origin': '*'},
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








        
import React from 'react';
import Chat from './components/Chat';

function App() {
  return (
    <div>
      <Chat />
    </div>
  );
}

export default App;




import React, { useState, useEffect } from 'react';
import './Chat.css';

const Chat = () => {
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');
  const [typing, setTyping] = useState(false);

  const websocketUrl = "wss://your-websocket-url"; // Replace with your WebSocket URL
  const httpEndpoint = "https://your-api-endpoint"; // Replace with your Lambda API Gateway endpoint

  useEffect(() => {
    const ws = new WebSocket(websocketUrl);

    ws.onopen = () => console.log('Connected to WebSocket');
    ws.onmessage = (event) => {
      const data = JSON.parse(event.data);
      setTyping(false);
      setMessages((prev) => [...prev, { type: 'bot', text: data.message }]);
    };

    ws.onclose = () => console.log('WebSocket connection closed');
    return () => ws.close();
  }, []);

  const sendMessage = () => {
    if (!input.trim()) return;
    setMessages((prev) => [...prev, { type: 'user', text: input }]);
    setTyping(true);
    setInput('');
    // Send message to WebSocket
  };

  const handleUpload = async (file) => {
    const reader = new FileReader();
    reader.onload = async (e) => {
      const base64 = e.target.result.split(',')[1];
      const response = await fetch(httpEndpoint, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ image: base64 }),
      });
      const data = await response.json();
      sendMessage(data.imageUrl);
    };
    reader.readAsDataURL(file);
  };

  return (
    <div className="chat-container">
      <div className="chat-header">HCLTech Insurance Agent - Demo</div>
      <div className="chat-messages">
        {messages.map((msg, index) => (
          <div key={index} className={msg.type === 'user' ? 'user-message' : 'bot-message'}>
            {msg.text}
          </div>
        ))}
        {typing && <div className="typing-indicator">Typing...</div>}
      </div>
      <input
        type="text"
        value={input}
        onChange={(e) => setInput(e.target.value)}
        placeholder="Type your message..."
      />
      <button onClick={sendMessage}>Send</button>
      <input
        type="file"
        id="image-upload"
        onChange={(e) => handleUpload(e.target.files[0])}
        style={{ display: 'none' }}
      />
      <button onClick={() => document.getElementById('image-upload').click()}>Upload Image</button>
    </div>
  );
};

export default Chat;
