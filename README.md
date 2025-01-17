def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'headers': {
            'Access-Control-Allow-Origin': 'https://29874210d84844ba8a40b817ea6de60c.vfs.cloud9.us-east-1.amazonaws.com',
            'Access-Control-Allow-Credentials': 'true',
            'Access-Control-Allow-Methods': 'GET, POST, OPTIONS',
            'Access-Control-Allow-Headers': 'Content-Type',
        },
        'body': json.dumps({'message': 'Success'})
    }





import os
import json
import base64
import boto3
from botocore.exceptions import ClientError

# Initialize the S3 client
s3 = boto3.client('s3')

def lambda_handler(event, context):
    try:
        # Log the received event for debugging
        print(f"Received event: {json.dumps(event)}")
        
        # Extract necessary values from the event
        http_method = event['requestContext']['http']['method']
        domain_name = event['requestContext']['domainName']
        stage = event['requestContext']['stage']
        http_endpoint = f"https://{domain_name}/{stage}/"
        
        # Retrieve environment variables
        websocket_url = os.environ.get('WEBSOCKET_URL')
        bucket_name = os.environ.get('IMAGE_BUCKET_SUBMITTED_BY_UI')

        # Ensure required environment variables are set
        if not websocket_url or not bucket_name:
            raise EnvironmentError("Missing required environment variables.")

        # Handle preflight requests for CORS
        if http_method == 'OPTIONS':
            return {
                'statusCode': 200,
                'headers': {
                    'Access-Control-Allow-Origin': '*',
                    'Access-Control-Allow-Methods': 'OPTIONS, POST, GET',
                    'Access-Control-Allow-Headers': 'Content-Type',
                },
            }

        # Handle POST requests
        if http_method == 'POST':
            body = json.loads(event.get('body', '{}'))  # Safely handle missing or invalid body
            image_data = body.get('image')

            if not image_data:
                return {
                    'statusCode': 400,
                    'headers': {'Access-Control-Allow-Origin': '*'},
                    'body': json.dumps({'message': 'Image data not provided'})
                }

            try:
                # Decode the base64 image data
                image_data = base64.b64decode(image_data)
            except base64.binascii.Error as decode_error:
                print(f"Base64 decode error: {decode_error}")
                return {
                    'statusCode': 400,
                    'headers': {'Access-Control-Allow-Origin': '*'},
                    'body': json.dumps({'message': 'Invalid base64 image data'})
                }

            # Generate a unique key for the image
            image_key = f"images/{context.aws_request_id}.png"

            try:
                # Upload the image to S3
                s3.put_object(
                    Bucket=bucket_name,
                    Key=image_key,
                    Body=image_data,
                    ContentType='image/png'
                )

                # Generate the public image URL
                image_url = f"https://{bucket_name}.s3.amazonaws.com/{image_key}"

                return {
                    'statusCode': 200,
                    'headers': {
                        'Content-Type': 'application/json',
                        'Access-Control-Allow-Origin': '*'
                    },
                    'body': json.dumps({'imageUrl': image_url})
                }
            except ClientError as e:
                print(f"S3 upload error: {e}")
                return {
                    'statusCode': 500,
                    'headers': {'Access-Control-Allow-Origin': '*'},
                    'body': json.dumps({'message': 'Error uploading image to S3'})
                }

        # Handle unsupported HTTP methods
        else:
            return {
                'statusCode': 405,
                'headers': {'Access-Control-Allow-Origin': '*'},
                'body': json.dumps({'message': 'Method not allowed'})
            }

    except Exception as e:
        print(f"Unhandled error: {e}")
        return {
            'statusCode': 500,
            'headers': {'Access-Control-Allow-Origin': '*'},
            'body': json.dumps({'message': 'Internal server error'})
        }








Access to fetch at 'api' from origin 'https://29874210d84844ba8a40b817ea6de60c.vfs.cloud9.us-east-1.amazonaws.com' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: The value of the 'Access-Control-Allow-Origin' header in the response must not be the wildcard '*' when the request's credentials mode is 'include'.


import React, { useState, useEffect, useRef, useCallback } from 'react';
import { BeatLoader } from 'react-spinners';
import { motion, AnimatePresence } from 'framer-motion';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faUpload, faRobot, faUser, faChevronRight } from '@fortawesome/free-solid-svg-icons';
import TracePanel from './TracePanel';
import './Chat.css';

const Chat = () => {
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');
  const [typing, setTyping] = useState(false);
  const [traceData, setTraceData] = useState([]);
  const [isTraceEnabled, setIsTraceEnabled] = useState(false);
  const [isLoading, setIsLoading] = useState(false);
  const [sessionId, setSessionId] = useState(null);
    const [showTracePanel, setShowTracePanel] = useState(false);
    const messagesEndRef = useRef(null); // For auto-scrolling

  
  

  const websocketUrl = "api1";
  const httpEndpoint = "api2";
  const traceApiEndpoint = "api3";

  const wsRef = useRef(null);
  
  // Auto-scroll to the bottom
  useEffect(() => {
    messagesEndRef.current?.scrollIntoView({ behavior: 'smooth' });
  }, [messages, typing]);

  // Establish WebSocket connection
  useEffect(() => {
    // Create WebSocket connection
    wsRef.current = new WebSocket(websocketUrl);

    wsRef.current.onopen = () => {
      console.log('Connected to WebSocket');
    };

    wsRef.current.onmessage = (event) => {
      try {
        const data = JSON.parse(event.data);
        console.log('Received WebSocket message:', data);

        // Handle different message types
        if (data.type === 'bot_response') {
          setTyping(false);
          setMessages((prev) => [...prev, { type: 'bot', text: data.data }]);
          
          // Update session ID if provided
          if (data.sessionId) {
            setSessionId(data.sessionId);
          }
        }
      } catch (error) {
        console.error('Error parsing WebSocket message:', error);
      }
    };

    wsRef.current.onerror = (error) => {
      console.error('WebSocket Error:', error);
    };

    wsRef.current.onclose = () => {
      console.log('WebSocket connection closed');
    };

    // Cleanup on component unmount
    return () => {
      if (wsRef.current) {
        wsRef.current.close();
      }
    };
  }, [websocketUrl]);

  // Send message via WebSocket
  const sendMessage = useCallback(() => {
    if (!input.trim()) return;

    // Add user message
    setMessages((prev) => [...prev, { type: 'user', text: input }]);
    setTyping(true);

    // Prepare payload
    const payload = {
      action: 'sendmessage',
      inputText: input,
    };

    // Include session ID if available
    if (sessionId) {
      payload.sessionId = sessionId;
    }

    // Send message if WebSocket is open
    if (wsRef.current && wsRef.current.readyState === WebSocket.OPEN) {
      try {
        wsRef.current.send(JSON.stringify(payload));
      } catch (error) {
        console.error('Error sending message:', error);
        setTyping(false);
      }
    } else {
      console.warn('WebSocket is not open');
      setTyping(false);
    }

    // Clear input
    setInput('');
  }, [input, sessionId]);

  // Handle file upload
  // const handleUpload = async (file) => {
  //   const reader = new FileReader();
  //   reader.onload = async (e) => {
  //     const base64 = e.target.result.split(',')[1];
      
  //     try {
  //       const response = await fetch(httpEndpoint, {
  //         method: 'POST',
  //         headers: { 'Content-Type': 'application/json' },
  //         body: JSON.stringify({ image: base64 }),
  //       });
        
  //       const data = await response.json();
        
  //       // Send image URL as a message
  //       if (wsRef.current && wsRef.current.readyState === WebSocket.OPEN) {
  //         const payload = {
  //           action: 'sendmessage',
  //           inputText: data.imageUrl,
  //         };

  //         if (sessionId) {
  //           payload.sessionId = sessionId;
  //         }

  //         wsRef.current.send(JSON.stringify(payload));
          
  //         // Add user message about image upload
  //         setMessages((prev) => [
  //           ...prev, 
  //           { type: 'user', text: 'Image uploaded' }
  //         ]);
  //         setTyping(true);
  //       }
  //     } catch (error) {
  //       console.error('Error uploading image:', error);
  //     }
  //   };
  //   reader.readAsDataURL(file);
  // };
  
  const handleUpload = async (file) => {
  const reader = new FileReader();
  
  reader.onload = async (e) => {
    const base64 = e.target.result.split(',')[1];
    
    try {
      const response = await fetch(httpEndpoint, {
        method: 'POST',
        mode: 'cors', // Add this line
        credentials: 'include', // Optional
        headers: {
          'Content-Type': 'application/json',
          'Accept': 'application/json',
          // Add origin header if needed
          'Origin': 'https://29874210d84844ba8a40b817ea6de60c.vfs.cloud9.us-east-1.amazonaws.com'
        },
        body: JSON.stringify({
          image: base64,
          filename: file.name,
          fileType: file.type,
        }),
      });

      // Comprehensive error handling
      if (!response.ok) {
        const errorText = await response.text();
        throw new Error(`HTTP error! status: ${response.status}, message: ${errorText}`);
      }
      
      const data = await response.json();

      
      // Validate response
      if (!data.imageUrl) {
        throw new Error('No image URL received');
      }
      
      
      // Send image URL via WebSocket
      if (wsRef.current && wsRef.current.readyState === WebSocket.OPEN) {
        const payload = {
          action: 'sendmessage',
          inputText: data.imageUrl,
          sessionId: sessionId || null, // Use null if no sessionId
        };

        wsRef.current.send(JSON.stringify(payload));
        
        // Add user message about image upload
        setMessages((prev) => [
          ...prev, 
          { 
            type: 'user', 
            text: 'Image uploaded', 
            imageUrl: data.imageUrl 
          }
        ]);
        
        setTyping(true);
      } else {
        console.warn('WebSocket is not open. Unable to send image message.');
      }
    
    } catch (error) {
      console.error('Error uploading image:', error);
      
      // User-friendly error handling
      const errorMessage = error.message || 'Failed to upload image';
      setMessages((prev) => [
        ...prev, 
        { 
          type: 'user', 
          text: `Error: \${errorMessage}` 
        }
      ]);
    }
  };

  // Error handling for FileReader
  reader.onerror = (error) => {
    console.error('Error reading file:', error);
    alert('Failed to read file. Please try again.');
  };

  // Trigger file reading
  reader.readAsDataURL(file);
};
  
    // Modify fetchTraceData function
//   const toggleTraceData = async () => {
//     // If panel is already showing, just hide it
//     if (showTracePanel) {
//       setShowTracePanel(false);
//       return;
//     }

// }

  // Fetch trace data
  const toggleTraceData = async () => {
     if (showTracePanel) {
      setShowTracePanel(false);
      return;
    }
    setIsLoading(true);
    try {
      const response = await fetch(traceApiEndpoint, {
        method: 'GET',
        headers: { Accept: 'application/json' },
      });

      if (!response.ok) {
        throw new Error('Failed to fetch trace data');
      }

      const data = await response.json();
      setTraceData(data);
      setIsTraceEnabled(true);
            setShowTracePanel(true);  // Show trace panel
    } catch (error) {
      console.error('Error fetching trace data:', error);
      setTraceData([]);
            setShowTracePanel(false);
    } finally {
      setIsLoading(false);
    }
  };

  // Handle key press for sending message
  const handleKeyPress = (event) => {
    if (event.key === 'Enter') {
      sendMessage();
    }
  };


  return (
    <div className="chat-wrapper">
      {/* Left Panel - Chatbot */}
      <div className="chatbot-panel">
        <div className="chat-header">
  <FontAwesomeIcon icon={faRobot} className="header-icon" />
  Insurance Claim Assist 
  <span className="powered-by">Powered by Agentic AI</span>
</div>
        <div className="chat-messages">
          <AnimatePresence>
            {messages.map((msg, index) => (
              <motion.div
                key={index}
                className={`message ${msg.type === 'user' ? 'user-message' : 'bot-message'}`}
                initial={{ opacity: 0, y: 20 }}
                animate={{ opacity: 1, y: 0 }}
                transition={{ duration: 0.3 }}
              >
                <div className="message-icon">
                  <FontAwesomeIcon icon={msg.type === 'user' ? faUser : faRobot} className={`${msg.type}-icon`} />
                </div>
                <div className="message-text">{msg.text}</div>
              </motion.div>
            ))}
          </AnimatePresence>

          {typing && (
            <div className="typing-indicator">
              <BeatLoader color="#785ce5" size={12} />
            </div>
          )}
                    <div ref={messagesEndRef} />

        </div>

        <div className="chat-input-container">
          <div className="input-wrapper">
            <input
              type="text"
              value={input}
              onChange={(e) => setInput(e.target.value)}
              onKeyDown={handleKeyPress}
              placeholder="Type your message..."
              className="chat-input"
            />
            <button onClick={sendMessage} className="btn btn-send">
              <FontAwesomeIcon icon={faPaperPlane} />
            </button>
          </div>
          <div className="button-group">
            <input
              type="file"
              id="image-upload"
              onChange={(e) => handleUpload(e.target.files[0])}
              style={{ display: 'none' }}
            />
            <button onClick={() => document.getElementById('image-upload').click()} className="btn btn-secondary">
              <FontAwesomeIcon icon={faUpload} className="btn-icon" />
              Upload
            </button>
                      <button 
            onClick={toggleTraceData} 
            className="btn btn-trace" 
            disabled={isLoading}
          >
            {isLoading ? (
              <BeatLoader color="#fff" size={12} />
            ) : showTracePanel ? (
              <>
                Hide Trace 
                <FontAwesomeIcon 
                  icon={faChevronRight} 
                  className="btn-icon right-chevron" 
                />
              </>
            ) : (
              <>
                Enable Trace 
                <FontAwesomeIcon 
                  icon={faChevronRight} 
                  className="btn-icon right-chevron" 
                />
              </>
            )}
          </button>

          </div>
        </div>
      </div>

      {/* Trace Panel */}
           {showTracePanel && (
        <TracePanel 
          isLoading={isLoading}
          traceData={traceData}
          isTraceEnabled={isTraceEnabled}
          onClose={() => setShowTracePanel(false)}
        />
      )}

    </div>
  );
};

export default Chat;




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




