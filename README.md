import React, { useState, useEffect, useRef, useCallback } from 'react';
import { BeatLoader } from 'react-spinners';
import { motion, AnimatePresence } from 'framer-motion';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faUpload, faRobot, faUser } from '@fortawesome/free-solid-svg-icons';
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
  const [isSending, setIsSending] = useState(false); // For send button loader

  const chatEndRef = useRef(null); // Reference for auto-scrolling
  const wsRef = useRef(null);

  const websocketUrl = "dummy";
  const httpEndpoint = "dummy2";
  const traceApiEndpoint = "dummy3";

  useEffect(() => {
    wsRef.current = new WebSocket(websocketUrl);

    wsRef.current.onopen = () => console.log('Connected to WebSocket');

    wsRef.current.onmessage = (event) => {
      try {
        const data = JSON.parse(event.data);
        if (data.type === 'bot_response') {
          setTyping(false);
          setMessages((prev) => [...prev, { type: 'bot', text: data.data }]);
          if (data.sessionId) setSessionId(data.sessionId);
        }
      } catch (error) {
        console.error('Error parsing WebSocket message:', error);
      }
    };

    wsRef.current.onerror = (error) => console.error('WebSocket Error:', error);
    wsRef.current.onclose = () => console.log('WebSocket connection closed');

    return () => wsRef.current?.close();
  }, [websocketUrl]);

  useEffect(() => {
    // Auto-scroll to the latest message
    chatEndRef.current?.scrollIntoView({ behavior: 'smooth' });
  }, [messages]);

  const sendMessage = useCallback(() => {
    if (!input.trim()) return;

    setMessages((prev) => [...prev, { type: 'user', text: input }]);
    setTyping(true);
    setIsSending(true);

    const payload = { action: 'sendmessage', inputText: input };
    if (sessionId) payload.sessionId = sessionId;

    if (wsRef.current?.readyState === WebSocket.OPEN) {
      wsRef.current.send(JSON.stringify(payload));
    } else {
      console.warn('WebSocket is not open');
      setTyping(false);
    }

    setInput('');
    setIsSending(false);
  }, [input, sessionId]);

  const handleKeyPress = (event) => {
    if (event.key === 'Enter') sendMessage();
  };

  const fetchTraceData = async () => {
    setIsLoading(true);
    try {
      const response = await fetch(traceApiEndpoint, {
        method: 'GET',
        headers: { Accept: 'application/json' },
      });
      const data = await response.json();
      setTraceData(data);
      setIsTraceEnabled(true);
    } catch (error) {
      console.error('Error fetching trace data:', error);
    } finally {
      setIsLoading(false);
    }
  };

  return (
    <div className="chat-wrapper">
      <div className="chatbot-panel">
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
                  <FontAwesomeIcon icon={msg.type === 'user' ? faUser : faRobot} />
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
          <div ref={chatEndRef} />
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
            <button onClick={sendMessage} className="btn btn-send" disabled={isSending}>
              {isSending ? <BeatLoader color="#fff" size={8} /> : <FontAwesomeIcon icon={faPaperPlane} />}
            </button>
          </div>
          <div className="button-group">
            <button onClick={fetchTraceData} className="btn btn-trace" disabled={isLoading}>
              {isLoading ? <BeatLoader color="#fff" size={12} /> : 'Enable Trace'}
            </button>
          </div>
        </div>
      </div>
    </div>
  );
};

export default Chat;












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

      {/* Trace Panel */}
      <TracePanel 
        isLoading={isLoading}
        traceData={traceData}
        isTraceEnabled={isTraceEnabled}
      />
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
  /*flex: 3;*/
  max-width: 400px;
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
  width: 100%;
  max-width: 960px;
  background: #ffffff;
  border-radius: 16px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  display: flex;
  flex-direction: row;
  max-height: 700px;
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

.chat-header {    background:
linear-gradient(304deg,#499dfd 3.03%,#2678f5 27.62%,#6628c5 76.39%,#5b0bb1 112.44%)
;

  color: #ffffff;
  padding: 15px;
  text-align: center;
  font-weight: 600;
  border-radius: 10px;
  margin-bottom: 15px;
  font-size:16px ;
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
  border: 1px solid #e0e0e0;
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
