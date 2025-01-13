import React, { useState, useEffect } from 'react';
import { BeatLoader } from 'react-spinners';
import { motion, AnimatePresence } from 'framer-motion';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faUpload, faRedo, faRobot, faUser } from '@fortawesome/free-solid-svg-icons';
import './Chat.css';

const Chat = () => {
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');
  const [typing, setTyping] = useState(false);
  const [traceData, setTraceData] = useState([]);
  const [isTraceEnabled, setIsTraceEnabled] = useState(false);
  const [isLoading, setIsLoading] = useState(false);

  const websocketUrl = "dummy";
  const httpEndpoint = "dummy1";
  const traceApiEndpoint = "dummy2";

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

  const fetchTraceData = async () => {
    setIsLoading(true);
    try {
      const response = await fetch(traceApiEndpoint, {
        method: 'GET',
        headers: { 'Accept': 'application/json' },
      });

      if (!response.ok) {
        throw new Error('Failed to fetch trace data');
      }

      const data = await response.json();
      setTraceData(data);
      setIsTraceEnabled(true);
    } catch (error) {
      console.error('Error fetching trace data:', error);
      setTraceData([]);
    } finally {
      setIsLoading(false);
    }
  };

  const handleKeyPress = (event) => {
    if (event.key === 'Enter') {
      sendMessage();
    }
  };

  return (
    <div className="chat-wrapper">
      <div className="chat-container">
        <div className="chat-content">
          <div className="chat-messages-container">
            <div className="chat-header">
              <FontAwesomeIcon icon={faRobot} className="header-icon" />
              HCLTech Insurance Agent - Demo
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
                      <FontAwesomeIcon 
                        icon={msg.type === 'user' ? faUser : faRobot} 
                        className={`${msg.type}-icon`} 
                      />
                    </div>
                    <div className="message-text">
                      {msg.text}
                    </div>
                  </motion.div>
                ))}
              </AnimatePresence>

              {typing && (
                <div className="typing-indicator">
                  <BeatLoader color="#785ce5" size={20} />
                </div>
              )}
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
                <div className="input-icons">
                  <button 
                    onClick={sendMessage} 
                    className="btn btn-send"
                  >
                    <FontAwesomeIcon icon={faPaperPlane} />
                  </button>
                </div>
              </div>
              <div className="button-group">
                <input
                  type="file"
                  id="image-upload"
                  onChange={(e) => handleUpload(e.target.files[0])}
                  style={{ display: 'none' }}
                />
                <button 
                  onClick={() => document.getElementById('image-upload').click()}
                  className="btn btn-secondary"
                >
                  <FontAwesomeIcon icon={faUpload} className="btn-icon" />
                  Upload
                </button>
                <button 
                  onClick={fetchTraceData} 
                  className="btn btn-trace"
                  disabled={isLoading}
                >
                  {isLoading ? (
                    <BeatLoader color="#fff" size={15} />
                  ) : (
                    <>
                      <FontAwesomeIcon icon={faRedo} className="btn-icon" />
                      {isTraceEnabled ? 'Refresh Trace' : 'Enable Trace'}
                    </>
                  )}
                </button>
              </div>
            </div>
          </div>

          {/* Trace Panel */}
          <AnimatePresence>
            {isTraceEnabled && (
              <motion.div 
                className="trace-panel"
                initial={{ width: 0, opacity: 0 }}
                animate={{ width: '300px', opacity: 1 }}
                exit={{ width: 0, opacity: 0 }}
                transition={{ duration: 0.3 }}
              >
                <div className="trace-header">
                  <h3>Agent Trace Steps</h3>
                </div>
                <div className="trace-content">
                  {isLoading ? (
                    <div className="trace-loading">
                      <BeatLoader color="#785ce5" size={30} />
                    </div>
                  ) : traceData.length > 0 ? (
                    traceData.map((step, index) => (
                      <motion.div 
                        key={index} 
                        className="trace-step"
                        initial={{ opacity: 0, y: 20 }}
                        animate={{ opacity: 1, y: 0 }}
                        transition={{ delay: index * 0.1 }}
                      >
                        <div className="trace-step-header">
                          Step {index + 1}
                        </div>
                        <pre>{JSON.stringify(step, null, 2)}</pre>
                      </motion.div>
                    ))
                  ) : (
                    <p className="no-trace">No trace data available</p>
                  )}
                </div>
              </motion.div>
            )}
          </AnimatePresence>
        </div>
      </div>
    </div>
  );
};

export default Chat;
