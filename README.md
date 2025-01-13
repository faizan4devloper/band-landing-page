
import React, { useState, useEffect } from 'react';
import { BeatLoader } from 'react-spinners';
import { motion, AnimatePresence } from 'framer-motion';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { 
  faPaperPlane, 
  faUpload, 
  faRedo, 
  faRobot, 
  faUser, 
  faTraceIcon 
} from '@fortawesome/free-solid-svg-icons';

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
      console.log('Raw Trace API Response:', data);
      
      setTraceData(data);
      setIsTraceEnabled(true);
    } catch (error) {
      console.error('Error fetching trace data:', error);
      setTraceData([]);
    } finally {
      setIsLoading(false);
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
                    className={`message \${msg.type === 'user' ? 'user-message' : 'bot-message'}`}
                    initial={{ opacity: 0, y: 20 }}
                    animate={{ opacity: 1, y: 0 }}
                    transition={{ duration: 0.3 }}
                  >
                    <div className="message-icon">
                      <FontAwesomeIcon 
                        icon={msg.type === 'user' ? faUser : faRobot} 
                        className={`\${msg.type}-icon`} 
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






@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600&display=swap');

:root {
  --primary-color: #785ce5;
  --secondary-color: #6a4fa3;
  --background-light: #f4f4f8;
  --white: #ffffff;
  --text-color: #333;
  --border-color: #e0e0e0;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Inter', sans-serif;
  background-color: var(--background-light);
  line-height: 1.6;
  color: var(--text-color);
}

.chat-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  padding: 20px;
  background-color: var(--background-light);
}

.chat-container {
  width: 900px;
  background-color: var(--white);
  border-radius: 16px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  display: flex;
  max-height: 700px;
}

.chat-content {
  display: flex;
  width: 100%;
}

.chat-messages-container {
  flex: 2;
  display: flex;
  flex-direction: column;
  border-right: 1px solid var(--border-color);
}

.chat-header {
  background-color: var(--primary-color);
  color: var(--white);
  padding: 15px;
  text-align: center;
  font-weight: 600;
  font-size: 18px;
}

.chat-messages {
  flex-grow: 1;
  overflow-y: auto;
  padding: 15px;
  background-color: #f9f9fc;
}

.message {
  max-width: 80%;
  margin-bottom: 10px;
  padding: 10px 15px;
  border-radius: 12px;
  clear: both;
  word-wrap: break-word;
  position: relative;
}

.user-message {
  background-color: var(--primary-color);
  color: var(--white);
  align-self: flex-end;
  margin-left: auto;
}

.bot-message {
  background-color: #e6e6f3;
  color: var(--text-color);
  align-self: flex-start;
}

.chat-input-container {
  padding: 15px;
  background-color: var(--white);
  border-top: 1px solid var(--border-color);
}

.chat-input {
  width: 100%;
  padding: 12px;
  border: 1px solid var(--border-color);
  border-radius: 8px;
  font-size: 16px;
  margin-bottom: 10px;
}

.button-group {
  display: flex;
  gap: 10px;
}

.btn {
  flex: 1;
  padding: 12px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-weight: 600;
  transition: all 0.3s ease;
  display: flex;
  justify-content: center;
  align-items: center;
}

.btn-primary {
  background-color: var(--primary-color);
  color: var(--white);
}

.btn-secondary {
  background-color: #e6e6f3;
  color: var(--primary-color);
}

.btn-trace {
  background-color: var(--secondary-color);
  color: var(--white);
}

.btn:hover {
  opacity: 0.9;
}

.btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.typing-indicator {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 10px 0;
}

.trace-panel {
  flex: 1;
  background-color: #f9f9fc;
  overflow-y: auto;
  max-width: 350px;
  border-left: 1px solid var(--border-color);
}

.trace-header {
  background-color: var(--primary-color);
  color: var(--white);
  padding: 15px;
  text-align: center;
  font-weight: 600;
}

.trace-content {
  padding: 15px;
}

.trace-step {
  background-color: var(--white);
  border: 1px solid var(--border-color);
  border-radius: 8px;
  margin-bottom: 10px;
  padding: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
}

.trace-step-header {
  font-weight: 600;
  color: var(--primary-color);
  margin-bottom: 5px;
}

.trace-step pre {
  font-family: 'Courier New', monospace;
  font-size: 12px;
  white-space: pre-wrap;
  word-break: break-all;
  background-color: #f4f4f8;
  padding: 10px;
  border-radius: 6px;
  max-height: 200px;
  overflow-y: auto;
}

.no-trace {
  text-align: center;
  color: #888;
  font-style: italic;
}

.trace-loading {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
}

/* Scrollbar Styling */
::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-track {
  background: #f1f1f1;
}

::-webkit-scrollbar-thumb {
  background: var(--primary-color);
  border-radius: 4px;
}

::-webkit-scrollbar-thumb:hover {
  background: var(--secondary-color);
}

/* Responsive Design */
@media (max-width: 768px) {
  .chat-container {
    flex-direction: column;
    width: 100%;
    max-height: none;
  }

  .chat-content {
    flex-direction: column;
  }

  .chat-messages-container,
  .trace-panel {
    flex: none;
    width: 100%;
    max-width: none;
    border-right: none;
    border-bottom: 1px solid var(--border-color);
  }
}


.header-icon {
  margin-right: 10px;
  color: var(--white);
}

.message {
  display: flex;
  align-items: flex-start;
  margin-bottom: 15px;
}

.message-icon {
  margin-right: 10px;
  display: flex;
  align-items: center;
}

.user-icon {
  color: var(--white);
  background-color: var(--primary-color);
  padding: 8px;
  border-radius: 50%;
}

.bot-icon {
  color: var(--primary-color);
  background-color: #e6e6f3;
  padding: 8px;
  border-radius: 50%;
}

.message-text {
  flex-grow: 1;
}

.input-wrapper {
  position: relative;
  display: flex;
  align-items: center;
}

.input-icons {
  position: absolute;
  right: 10px;
}

.btn-send {
  background: none;
  border: none;
  color: var(--primary-color);
  cursor: pointer;
}

.btn-icon {
  margin-right: 8px;
}

.button-group .btn {
  display: flex;
  align-items: center;
  justify-content: center;
}

