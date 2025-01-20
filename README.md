const toggleTraceData = async (index) => {
  // Toggle trace panel visibility for clicked message
  setMessages((prevMessages) =>
    prevMessages.map((msg, i) => ({
      ...msg,
      showTracePanel: i === index ? !msg.showTracePanel : msg.showTracePanel,
      isTraceLoading: i === index ? true : msg.isTraceLoading, // Start loading state
    }))
  );

  // If the trace panel is enabled, fetch trace data
  if (!showTracePanel) {
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
                onClick={() => toggleTraceData(index)} // Pass index here
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

