const toggleTraceData = async (index) => {
  setIsTraceLoading(true); // Start loading

  // Update specific message's loading state
  setMessages((prevMessages) =>
    prevMessages.map((msg, i) =>
      i === index ? { ...msg, isTraceLoading: true } : msg
    )
  );

  try {
    const response = await fetch(traceApiEndpoint, {
      method: 'GET',
      headers: { Accept: 'application/json' },
    });

    if (!response.ok) throw new Error('Trace fetch failed');

    const data = await response.json();
    setTraceData(data);
    setIsTraceEnabled(true);

    // Update specific message's trace panel and loading state
    setMessages((prevMessages) =>
      prevMessages.map((msg, i) =>
        i === index
          ? { ...msg, isTraceLoading: false, showTracePanel: !msg.showTracePanel }
          : msg
      )
    );
  } catch (error) {
    console.error('Trace error:', error);

    // Reset loading state even on error
    setMessages((prevMessages) =>
      prevMessages.map((msg, i) =>
        i === index ? { ...msg, isTraceLoading: false } : msg
      )
    );
  } finally {
    setIsTraceLoading(false); // Reset global loading state
  }
};





import React, { useState, useEffect, useRef, useCallback } from 'react';
import { BeatLoader } from 'react-spinners';
import { motion, AnimatePresence } from 'framer-motion';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faPlus, faRobot, faUser, faChevronRight } from '@fortawesome/free-solid-svg-icons';
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
const [isUploadLoading, setIsUploadLoading] = useState(false);
const [isTraceLoading, setIsTraceLoading] = useState(false);

  
  

  const websocketUrl = "dummy";
 
  const httpEndpoint = "dummy";
  
  const traceApiEndpoint = "dummy";

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
      setIsUploadLoading(true); // Set upload loading state
      const response = await fetch(httpEndpoint, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          Accept: 'application/json',
        },
        body: JSON.stringify({
          image: base64,
          filename: file.name,
          fileType: file.type,
        }),
      });

      if (!response.ok) throw new Error('Upload failed');

      const data = await response.json();

      if (!data.imageUrl) throw new Error('Invalid upload response');

      if (wsRef.current && wsRef.current.readyState === WebSocket.OPEN) {
        wsRef.current.send(JSON.stringify({
          action: 'sendmessage',
          inputText: data.imageUrl,
          sessionId: sessionId || null,
        }));

        setMessages((prev) => [
          ...prev,
          { type: 'user', text: 'Image uploaded', imageUrl: data.imageUrl },
        ]);
        setTyping(true);
      } else {
        console.warn('WebSocket not open');
      }
    } catch (error) {
      console.error('Upload error:', error);
      setMessages((prev) => [
        ...prev,
        { type: 'user', text: `Error: ${error.message}` },
      ]);
    } finally {
      setIsUploadLoading(false); // Reset upload loading state
    }
  };

  reader.onerror = (error) => {
    console.error('File reading error:', error);
    alert('Failed to read file');
  };

  reader.readAsDataURL(file);
};

const toggleTraceData = async (index) => {
  
  setMessages((prevMessages) =>
     prevMessages.map((msg, i) => ({
      ...msg,
      showTracePanel: i === index ? !msg.showTracePanel : false, // Only toggle the clicked one
    }))
      
    
  );

  // Simulate loading time
  setTimeout(() => {
    setMessages((prevMessages) =>
      prevMessages.map((msg, i) =>
        i === index
          ? {
              ...msg,
              isTraceLoading: false,
              showTracePanel: !msg.showTracePanel, // Toggle trace panel for the selected message
            }
          : msg
      )
    );
  }, 1000); 
  if (showTracePanel) {
    setShowTracePanel(false);
    return;
  }
  setIsTraceLoading(true); // Set trace loading state
  try {
    const response = await fetch(traceApiEndpoint, {
      method: 'GET',
      headers: { Accept: 'application/json' },
    });

    if (!response.ok) throw new Error('Trace fetch failed');

    const data = await response.json();
    setTraceData(data);
    setIsTraceEnabled(true);
    setShowTracePanel(true);
  } catch (error) {
    console.error('Trace error:', error);
    setTraceData([]);
    setShowTracePanel(false);
  } finally {
    setIsTraceLoading(false); // Reset trace loading state
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
        <div className="message-text">
          {msg.text}

          {msg.type === 'bot' && (
            <span className="trace-button-container">
              <button
  onClick={() => toggleTraceData(index)}
  className={`btn btn-trace ${msg.showTracePanel ? 'active' : ''}`}
  disabled={msg.isTraceLoading}
>
  {msg.isTraceLoading ? (
    <BeatLoader color="#fff" size={12} />
  ) : msg.showTracePanel ? (
    'Hide Trace'
  ) : (
    'Enable Trace'
  )}
</button>
            </span>
          )}
        </div>
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
  <div className="input-icons">
      <FontAwesomeIcon 
        icon={faPlus} 
        className="upload-icon" 
        onClick={() => document.getElementById('image-upload').click()}
      />
    </div>
    <input
      type="text"
      value={input}
      onChange={(e) => setInput(e.target.value)}
      onKeyDown={handleKeyPress}
      placeholder="Type your message..."
      className="chat-input"
    />
    <input
      type="file"
      id="image-upload"
      onChange={(e) => handleUpload(e.target.files[0])}
      style={{ display: 'none' }}
    />
    
      <button onClick={sendMessage} className="btn btn-send">
        <FontAwesomeIcon icon={faPaperPlane} />
      </button>
  </div>
          <div className="button-group">
          

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

.chat-wrapper {
  display: flex;
  min-height: 100vh;
  background-color: #f5f5f5;
  gap: 20px;
  padding: 20px;
}

.upload-panel {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 200px;
  background: #ffffff;
  border-radius: 16px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
  padding: 20px;
}

.upload-plus-icon {
  font-size: 40px;
  color: #5c6bc0;
  cursor: pointer;
}

.upload-wrapper p {
  margin-top: 10px;
  font-size: 16px;
  color: #424242;
}


.chatbot-panel,
.trace-panel {
  background: #ffffff;
  border-radius: 16px;
  padding: 20px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
}

.chatbot-panel {
  /*flex: 3;*/
  width: 600px;
  height: 90vh;
}

.trace-panel {
  flex: 2;
  height: 90vh;
}

/*.chat-header {*/
/*  font-size: 20px;*/
/*  font-weight: bold;*/
/*  margin-bottom: 15px;*/
/*  color: #1462dd;*/
/*}*/

.message {
  margin-bottom: 10px;
}

.bot-message .message-text {
  background: #e3f2fd;
}

.user-message .message-text {
  background: #fbfbfb;
}

.btn {
  background-color: #5c6bc0;
  color: white;
  margin: 5px 0;
}


.chat-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  padding: 20px;
  background: linear-gradient(135deg, #eceff1, #f5f5f5);
}

.chat-container {
  flex: 1;
  background: #ffffff;
  border-radius: 16px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
  padding: 20px;
  display: flex;
  flex-direction: column;
}

.chat-content {
  display: flex;
  flex: 1;
  overflow: hidden;
}

.chat-messages-container {
  flex: 2;
  display: flex;
  flex-direction: column;
  padding: 20px;
}

.chat-header {    
  background: linear-gradient(304deg, #499dfd 3.03%, #2678f5 27.62%, #6628c5 76.39%, #5b0bb1 112.44%);
  color: #ffffff;
  padding: 15px;
  text-align: center;
  font-weight: 600;
  border-radius: 10px;
  margin-bottom: 15px;
  font-size: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
}

.header-icon {
  font-size: 24px;
  /*margin-right: 10px;*/
}

.powered-by {
  position: relative;
  font-style: italic;
  font-size: 10px;
  transition: all 0.3s ease;
}



/*.powered-by::after {*/
/*  content: '';*/
/*  position: absolute;*/
/*  bottom: -2px;*/
/*  left: 0;*/
/*  width: 0;*/
/*  height: 2px;*/
/*  background: linear-gradient(to right, #ff9966, #ff5e62);*/
/*  transition: width 0.3s ease;*/
/*}*/

.powered-by:hover::after {
  width: 100%;
}

.chat-messages {
  flex: 1;
  overflow-y: auto;
  background-color: #f9f9fc;
  padding: 15px;
  border-radius: 10px;
}

.message {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
  animation: fadeIn 0.3s ease-in-out;
}

.user-message {
  justify-content: flex-start;
}

.bot-message {
  justify-content: flex-end;
}

.message-icon {
  margin-right: 10px;
}

.message-text {
  background-color: #e1f5fe;
  padding: 10px 15px;
  border-radius: 10px;
  font-size: 14px;
  max-width: 70%;
  word-wrap: break-word;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.trace-button-container {
  display: inline-block;
  margin-left: 10px;
}

.trace-button-container .btn-trace {
  padding: 5px 10px;
  font-size: 12px;
  background-color: #785ce5;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.trace-button-container .btn-trace:hover {
  background-color: #6041c7;
}


.user-message .message-text {
  background-color: #fbfbfb;
}

.bot-message .message-text {
  background-color: #bbdefb;
}

.chat-input-container {
  margin-top: 10px;
  display: flex;
  flex-direction: column;
}

.input-wrapper {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background-color: #ffffff;
  padding: 8px;
  border-radius: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.chat-input {
  flex: 1;
  padding: 10px;
  border-radius: 8px;
  /*border: 1px solid #e0e0e0;*/
  outline:none;
  border:none;
  margin-right: 10px;
  font-size: 16px;
}

.input-icons .btn {
  background: none;
  border: none;
  cursor: pointer;
  padding: 8px;
}

.input-icons .btn-send {
  background:
linear-gradient(304deg,#499dfd 3.03%,#2678f5 27.62%,#6628c5 76.39%,#5b0bb1 112.44%)
;
  color: #ffffff;
  border-radius: 50%;
  padding: 12px;
  font-size: 18px;
  transition: background-color 0.3s ease;
}

.input-icons .btn-send:hover {
  background-color: #3949ab;
}

.button-group {
  display: flex;
  justify-content: space-between;
  margin-top: 10px;
}

.btn {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px 20px;
  font-size: 14px;
  font-weight: 600;
  color: #ffffff;
  background:
linear-gradient(304deg,#499dfd 3.03%,#2678f5 27.62%,#6628c5 76.39%,#5b0bb1 112.44%)
;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.2s ease;
}

.btn-icon {
  margin-right: 8px;
}

.btn:hover {
  background-color: #3949ab;
}

.btn:active {
  transform: scale(0.95);
  background-color: #303f9f;
}

.btn-secondary {
  background-color: #26a69a;
}

.btn-secondary:hover {
  background-color: #1b8779;
}

.btn-secondary:active {
  background-color: #177466;
}

.btn-trace {
  background-color: #ff7043;
}

.btn-trace:hover {
  background-color: #ff5722;
}

.btn-trace:active {
  background-color: #e64a19;
}

.btn:disabled {
  background-color: #bdbdbd;
  cursor: not-allowed;
}

.trace-panel {
  width: 300px;
  padding: 20px;
  /*background-color: #5c6bc0;*/
  color: #000;
  overflow-y: auto;
  border-left: 1px solid #e0e0e0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
}

.trace-header h3 {
  font-size: 18px;
  margin-bottom: 10px;
}

.trace-content {
  width: 100%;
}

.trace-step {
  margin-bottom: 15px;
  background-color: #9fa8da;
  padding: 15px;
  border-radius: 8px;
  color: #ffffff;
}

.trace-step-header {
  font-weight: bold;
  margin-bottom: 5px;
}

.no-trace {
  text-align: center;
  color: #ddd;
}
.error-trace{
    text-align: center;


}

/*.input-wrapper {*/
/*  display: flex;*/
/*  align-items: center;*/
/*  position: relative;*/
/*  width: 100%;*/
/*}*/

.chat-input {
  flex-grow: 1;
  padding-right: 70px; /* Make space for icons */
}

.input-icons{
      display: flex
;
    align-items: center;
    justify-content: center;
    background:linear-gradient(304deg,#499dfd 3.03%,#2678f5 27.62%,#6628c5 76.39%,#5b0bb1 112.44%);
    border: none;
    color: white;
    padding: 7px 8px;
    border-radius: 50%;
    cursor: pointer;
    transition: all 0.3s ease;
}





@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}


.typing-indicator {
  display: flex;
  justify-content: flex-end;
  padding: 10px;
}

/*.btn-send {*/
/*  display: flex;*/
/*  align-items: center;*/
/*  justify-content: center;*/
/*  background-color: #5c6bc0;*/
/*  border: none;*/
/*  color: white;*/
/*  padding: 10px;*/
/*  border-radius: 50%;*/
/*  cursor: pointer;*/
/*  transition: all 0.3s ease;*/
/*}*/

.btn-send:disabled {
  background-color: #cccccc;
  cursor: not-allowed;
}
.right-chevron{
  padding-left: 5px;
}
.btn-trace {
  background-color: #ff7043;
  transition: all 0.3s ease;
}

.btn-trace.active {
  background-color: #e64a19;
}


.trace-button-container {
  margin-top: 5px;
  text-align: right;
}

.trace-button-container .btn-trace {
  padding: 5px 10px;
  font-size: 12px;
  background-color: #785ce5;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.trace-button-container .btn-trace:hover {
  background-color: #6041c7;
}

.trace-button-container .btn-trace.active {
  background-color: #ff6b6b;
}










