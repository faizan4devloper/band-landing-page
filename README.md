import React, { useState, useEffect, useRef, useCallback } from 'react';
import { BeatLoader } from 'react-spinners';
import { motion, AnimatePresence } from 'framer-motion';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faUpload, faRobot, faUser } from '@fortawesome/free-solid-svg-icons';
import './Chat.css';

const Chat = () => {
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');
  const [typing, setTyping] = useState(false);
  const [isLoading, setIsLoading] = useState(false);
  const messagesEndRef = useRef(null); // For auto-scrolling

  // WebSocket setup (omitted for brevity)

  // Auto-scroll to the bottom
  useEffect(() => {
    messagesEndRef.current?.scrollIntoView({ behavior: 'smooth' });
  }, [messages, typing]);

  const sendMessage = useCallback(() => {
    if (!input.trim()) return;

    setMessages((prev) => [...prev, { type: 'user', text: input }]);
    setTyping(true);
    setIsLoading(true); // Show loader on send button

    // Mock sending message
    setTimeout(() => {
      setTyping(false);
      setMessages((prev) => [...prev, { type: 'bot', text: 'Bot response here...' }]);
      setIsLoading(false); // Remove loader on send button
    }, 2000);

    setInput('');
  }, [input]);

  const handleKeyPress = (event) => {
    if (event.key === 'Enter') {
      sendMessage();
    }
  };

  return (
    <div className="chat-wrapper">
      <div className="chatbot-panel">
        <div className="chat-header">
          <FontAwesomeIcon icon={faRobot} className="header-icon" />
          Chatbot Demo
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
              disabled={isLoading}
            />
            <button onClick={sendMessage} className="btn btn-send" disabled={isLoading}>
              {isLoading ? <BeatLoader color="#fff" size={8} /> : <FontAwesomeIcon icon={faPaperPlane} />}
            </button>
          </div>
        </div>
      </div>
    </div>
  );
};

export default Chat;




useEffect(() => {
  messagesEndRef.current?.scrollIntoView({ behavior: 'smooth' });
}, [messages, typing]);


{typing && (
  <div className="typing-indicator">
    <BeatLoader color="#785ce5" size={12} />
  </div>
)}






<button onClick={sendMessage} className="btn btn-send" disabled={isLoading}>
  {isLoading ? <BeatLoader color="#fff" size={8} /> : <FontAwesomeIcon icon={faPaperPlane} />}
</button>






.typing-indicator {
  display: flex;
  justify-content: flex-end;
  padding: 10px;
}

.btn-send {
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #5c6bc0;
  border: none;
  color: white;
  padding: 10px;
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn-send:disabled {
  background-color: #cccccc;
  cursor: not-allowed;
}
