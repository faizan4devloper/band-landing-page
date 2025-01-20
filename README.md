

import React, { useState, useEffect, useRef, useCallback } from 'react';
import { BeatLoader } from 'react-spinners';
import { motion, AnimatePresence } from 'framer-motion';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faUpload, faRobot, faUser, faChevronRight, faPlusCircle, faComments } from '@fortawesome/free-solid-svg-icons';
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

  
const handleUpload = async (file) => {
  const reader = new FileReader();

  reader.onload = async (e) => {
    const base64 = e.target.result.split(',')[1];

    try {
      // Set loading state
      setIsLoading(true);

      const response = await fetch(httpEndpoint, {
        method: 'POST',
        // mode: 'cors',
        headers: {
          'Content-Type': 'application/json',
          'Accept': 'application/json',
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
          sessionId: sessionId || null,
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
          text: `Error: ${errorMessage}`
        }
      ]);
    } finally {
      // Reset loading state
      setIsLoading(false);
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

<div className="chat-wrapper">
  <div className="upload-panel">
    <input
      type="file"
      id="file-upload"
      onChange={(e) => handleUpload(e.target.files[0])}
      style={{ display: 'none' }}
    />
    <div className="upload-wrapper">
      <FontAwesomeIcon
        icon={faPlusCircle}
        className="upload-plus-icon"
        onClick={() => document.getElementById('file-upload').click()}
      />
      <p>Upload File</p>
    </div>
  </div>

  <div className="chat-container">
    <div className="chat-content">
      <div className="chat-messages-container">
        <div className="chat-header">
          <FontAwesomeIcon icon={faComments} className="header-icon" />
          Chat with AI Ninja
        </div>

        <div className="chat-messages">
          {/* Chat messages go here */}
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
            <button
              onClick={toggleTraceData}
              className={`btn btn-trace ${showTracePanel ? 'active' : ''}`}
              disabled={isLoading}
            >
              {isLoading ? (
                <BeatLoader color="#fff" size={12} />
              ) : showTracePanel ? (
                <>
                  Hide Trace
                  <FontAwesomeIcon icon={faChevronRight} className="btn-icon right-chevron" />
                </>
              ) : (
                <>
                  Enable Trace
                  <FontAwesomeIcon icon={faChevronRight} className="btn-icon right-chevron" />
                </>
              )}
            </button>
          </div>
        </div>
      </div>
    </div>
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

