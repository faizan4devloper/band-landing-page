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
        headers: { Accept: 'application/json' },
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
      {/* Left Panel - Chatbot */}
      <div className="chatbot-panel">
        <div className="chat-header">
          <FontAwesomeIcon icon={faRobot} className="header-icon" />
          AI Ninja - Chat
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
            <button onClick={fetchTraceData} className="btn btn-trace" disabled={isLoading}>
              {isLoading ? <BeatLoader color="#fff" size={12} /> : 'Enable Trace'}
            </button>
          </div>
        </div>
      </div>

      {/* Right Panel - Trace */}
      {isTraceEnabled && (
        <div className="trace-panel">
          <div className="trace-header">Trace Steps</div>
          <div className="trace-content">
            {traceData.length ? (
              traceData.map((step, index) => (
                <div key={index} className="trace-step">
                  <h4>Step {index + 1}</h4>
                  <pre>{JSON.stringify(step, null, 2)}</pre>
                </div>
              ))
            ) : (
              <p>No trace data available</p>
            )}
          </div>
        </div>
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
  flex: 3;
  height: 90vh;
}

.trace-panel {
  flex: 2;
  height: 90vh;
}

.chat-header {
  font-size: 20px;
  font-weight: bold;
  margin-bottom: 15px;
  color: #3949ab;
}

.message {
  margin-bottom: 10px;
}

.bot-message .message-text {
  background: #e3f2fd;
}

.user-message .message-text {
  background: #c8e6c9;
}

.btn {
  background-color: #5c6bc0;
  color: white;
  margin: 5px 0;
}
