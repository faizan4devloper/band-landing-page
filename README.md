import React, { useState, useEffect, useRef } from 'react';
import axios from 'axios';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faWandSparkles, faTrashAlt, faComments, faChevronDown } from '@fortawesome/free-solid-svg-icons';
import { faUser } from '@fortawesome/free-regular-svg-icons';
import { BeatLoader } from 'react-spinners';
import styles from './Chatbot.module.css';
import BotProfile from "./chatbotprofile.jpg";

const Chatbot = () => {
  const [isOpen, setIsOpen] = useState(false);
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');
  const [loading, setLoading] = useState(false);
  const [showClearChat, setShowClearChat] = useState(false);
  
  const messagesEndRef = useRef(null);

  useEffect(() => {
    if (isOpen && messages.length === 0) {
      setMessages([{ text: 'Hello! How can I assist you today?', sender: 'bot' }]);
    }
  }, [isOpen]);

  useEffect(() => {
    if (messagesEndRef.current) {
      messagesEndRef.current.scrollIntoView({ behavior: 'smooth' });
    }
  }, [messages]);

  const toggleChatbot = () => {
    setIsOpen(!isOpen);
  };

  const minimizeChatbot = () => {
    setIsOpen(false);
  };

  const sendMessage = async () => {
    if (input.trim() === '') return;

    const userMessage = { text: input, sender: 'user' };
    setMessages([...messages, userMessage]);
    setInput('');
    setLoading(true);

    setMessages((prevMessages) => [
      ...prevMessages,
      { text: '', sender: 'bot', loading: true },
    ]);

    try {
      const response = await axios.post('https://sognpdfh09.execute-api.us-east-1.amazonaws.com/v1', // Replace with your actual endpoint
        { query: input },
        {
          headers: {
            'Content-Type': 'application/json',
          },
        }
      );

      const botMessage = { text: response.data.text, sender: 'bot' };

      setMessages((prevMessages) =>
        prevMessages.map((msg, index) =>
          index === prevMessages.length - 1 ? botMessage : msg
        )
      );
    } catch (error) {
      console.error('Error sending message:', error);
      setMessages((prevMessages) => [
        ...prevMessages,
        { text: 'Sorry, something went wrong. Please try again later.', sender: 'bot' },
      ]);
    } finally {
      setLoading(false);
    }
  };

  const handleClearChat = () => {
    setMessages([]);
    setShowClearChat(false);
  };

  const linkify = (text) => {
    const urlPattern = /(https?:\/\/[^\s]+)/g;
    return text.split(urlPattern).map((part, index) =>
      urlPattern.test(part) ? (
        <a key={index} href={part} target="_blank" rel="noopener noreferrer">
          {part}
        </a>
      ) : (
        part
      )
    );
  };

  return (
    <>
      <div className={styles.chatbotIcon} onClick={toggleChatbot} title="AI Ninja Chat">
        <FontAwesomeIcon icon={faComments} />
      </div>

      {isOpen && (
        <div className={`${styles.chatbotContainer} ${isOpen ? styles.open : ''}`}>
          <div className={styles.chatbotHeader}>
            <div className={styles.botProfile}>
              <img src={BotProfile} className={styles.botImage} />
              <div className={styles.botInfo}>
                <div className={styles.botName}>AI Ninja</div>
                <div className={styles.botStatus}>
                  <span className={styles.onlineDot}></span> Online
                </div>
              </div>
            </div>
            <div className={styles.headerActions}>
              <button onClick={() => setShowClearChat(true)} className={styles.clearChatButton} title="Clear Chat">
                <FontAwesomeIcon icon={faTrashAlt} />
              </button>
              <button onClick={minimizeChatbot} className={styles.minimizeButton} title="Minimize Chat">
                <FontAwesomeIcon icon={faChevronDown} />
              </button>
            </div>
          </div>

          <div className={styles.chatbotMessages}>
            {messages.map((message, index) => (
              <div
                key={index}
                className={
                  message.sender === 'user' ? styles.userMessage : styles.botMessage
                }
              >
                <FontAwesomeIcon
                  icon={message.sender === 'user' ? faUser : faWandSparkles}
                  className={styles.icon}
                />
                <div className={styles.messageText}>
                  {message.loading ? (
                    <BeatLoader color="#5f1ec1" size={8} />
                  ) : (
                    linkify(message.text)
                  )}
                </div>
              </div>
            ))}
            <div ref={messagesEndRef} />
          </div>

          {showClearChat && (
            <div className={styles.clearChatWindow}>
              <p>Are you sure you want to clear the chat?</p>
              <button onClick={handleClearChat} className={styles.confirmButton}>
                Yes, Clear Chat
              </button>
              <button onClick={() => setShowClearChat(false)} className={styles.cancelButton}>
                Cancel
              </button>
            </div>
          )}

          <div className={styles.chatbotInput}>
            <input
              type="text"
              value={input}
              onChange={(e) => setInput(e.target.value)}
              onKeyDown={(e) => {
                if (e.key === "Enter") {
                  sendMessage();
                }
              }}
              placeholder="Type your message and hit Enter..."
              disabled={loading}
            />
            <button onClick={sendMessage} disabled={loading} className={styles.sendButton} title="Send">
              <FontAwesomeIcon icon={faPaperPlane} />
            </button>
          </div>
        </div>
      )}
    </>
  );
};

export default Chatbot;




/* ... previous styles ... */

.chatbotContainer {
  position: fixed;
  bottom: 80px;
  right: 20px;
  width: 320px;
  height: 420px;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.3);
  display: flex;
  flex-direction: column;
  z-index: 1001;
  opacity: 0;
  transform: scale(0.9);
  transition: opacity 0.3s ease, transform 0.3s ease;
}

.chatbotContainer.open {
  opacity: 1;
  transform: scale(1);
}

.clearChatWindow {
  position: absolute;
  bottom: 50px; /* Adjust based on available space */
  left: 0;
  right: 0;
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.3);
  text-align: center;
}

.confirmButton {
  background-color: #d9534f;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 10px;
}

.cancelButton {
  background-color: #5f1ec1;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

/* Rest of the styles */









import React, { useState, useEffect, useRef } from 'react';
import axios from 'axios';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faWandSparkles, faTrashAlt, faComments, faChevronDown } from '@fortawesome/free-solid-svg-icons';
import { faUser } from '@fortawesome/free-regular-svg-icons';
import { BeatLoader } from 'react-spinners';
import styles from './Chatbot.module.css';
import BotProfile from "./chatbotprofile.jpg";

const Chatbot = () => {
  const [isOpen, setIsOpen] = useState(false);
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');
  const [loading, setLoading] = useState(false);
  const [showClearChat, setShowClearChat] = useState(false);
  
  const messagesEndRef = useRef(null)

  useEffect(() => {
    if (isOpen && messages.length === 0) {
      setMessages([{ text: 'Hello! How can I assist you today?', sender: 'bot' }]);
    }
  }, [isOpen]);
  
  // Auto-scroll effect whenever messages change
  useEffect(() => {
    if (messagesEndRef.current) {
      messagesEndRef.current.scrollIntoView({ behavior: 'smooth' });
    }
  }, [messages]);

  const toggleChatbot = () => {
    setIsOpen(!isOpen);
    
  };
  
  const minimizeChatbot = () => {
    setIsOpen(false);
  };

  const sendMessage = async () => {
    if (input.trim() === '') return;

    const userMessage = { text: input, sender: 'user' };
    setMessages([...messages, userMessage]);
    setInput('');
    setLoading(true);

    setMessages((prevMessages) => [
      ...prevMessages,
      { text: '', sender: 'bot', loading: true },
    ]);

    try {
      const response = await axios.post('https://sognpdfh09.execute-api.us-east-1.amazonaws.com/v1', // This is Haiku 3 Api
      // const response = await axios.post('https://7ss0r1iqfc.execute-api.us-east-1.amazonaws.com/v1', // Replace with your actual endpoint
        { query: input },
        {
          headers: {
            'Content-Type': 'application/json',
          },
        }
      );

      const botMessage = { text: response.data.text, sender: 'bot' };

      setMessages((prevMessages) =>
        prevMessages.map((msg, index) =>
          index === prevMessages.length - 1 ? botMessage : msg
        )
      );
    } catch (error) {
      console.error('Error sending message:', error);
      setMessages((prevMessages) => [
        ...prevMessages,
        { text: 'Sorry, something went wrong. Please try again later.', sender: 'bot' },
      ]);
    } finally {
      setLoading(false);
    }
  };

  const handleClearChat = () => {
    setMessages([]);
    setShowClearChat(false);
  };

  const linkify = (text) => {
    const urlPattern = /(https?:\/\/[^\s]+)/g;
    return text.split(urlPattern).map((part, index) =>
      urlPattern.test(part) ? (
        <a key={index} href={part} target="_blank" rel="noopener noreferrer">
          {part}
        </a>
      ) : (
        part
      )
    );
  };

  return (
    <>
      <div className={styles.chatbotIcon} onClick={toggleChatbot} title="AI Ninja Chat">
        <FontAwesomeIcon icon={faComments} />
      </div>

      {isOpen && (
        <div className={`${styles.chatbotContainer} ${isOpen ? styles.open : ''}`}>
          <div className={styles.chatbotHeader}>
            <div className={styles.botProfile}>
              <img src={BotProfile} className={styles.botImage} />
              <div className={styles.botInfo}>
                <div className={styles.botName}>AI Ninja</div>
                <div className={styles.botStatus}><span className={styles.greenDot}></span> Online</div>
              </div>
            </div>
            <div className={styles.headerActions}>
              
              <button onClick={() => setShowClearChat(true)} className={styles.clearChatButton} title="Clear Chat">
                <FontAwesomeIcon icon={faTrashAlt} />
              </button>
              <button onClick={minimizeChatbot} className={styles.minimizeButton} title="Minimize Chat">
                <FontAwesomeIcon icon={faChevronDown} />
              </button>
            </div>
          </div>

          <div className={styles.chatbotMessages}>
            {messages.map((message, index) => (
              <div
                key={index}
                className={
                  message.sender === 'user' ? styles.userMessage : styles.botMessage
                }
              >
                <FontAwesomeIcon
                  icon={message.sender === 'user' ? faUser : faWandSparkles}
                  className={styles.icon}
                />
                <div className={styles.messageText}>
                  {message.loading ? (
                    <BeatLoader color="#5f1ec1" size={8} />
                  ) : (
                    linkify(message.text)
                  )}
                </div>
              </div>
            ))}
            <div ref={messagesEndRef}/>
          </div>

          <div className={styles.chatbotInput}>
            <input
              type="text"
              value={input}
              onChange={(e) => setInput(e.target.value)}
              onKeyDown={(e) => {
                if (e.key === "Enter") {
                  sendMessage();
                }
              }}
              placeholder="Type your message and hit Enter..."
              disabled={loading}
            />
            <button onClick={sendMessage} disabled={loading} className={styles.sendButton} title="Enter">
              <FontAwesomeIcon icon={faPaperPlane} />
            </button>
          </div>
        </div>
      )}

      {showClearChat && (
        <div className={styles.clearChatOverlay}>
          <div className={styles.clearChatWindow}>
            <p>Are you sure you want to clear the chat?</p>
            <button onClick={handleClearChat} className={styles.confirmButton}>
              Yes, Clear Chat
            </button>
            <button onClick={() => setShowClearChat(false)} className={styles.cancelButton}>
              Cancel
            </button>
          </div>
        </div>
      )}
    </>
  );
};

export default Chatbot;

.chatbotIcon {
  position: fixed;
  bottom: 20px;
  right: 20px;
background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);  color: white;
  padding: 10px;
  border-radius: 50%;
  cursor: pointer;
  z-index: 1000;
  animation: float 3s ease-in-out infinite;
}

@keyframes float {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-10px);
  }
}

.chatbotContainer {
  position: fixed;
  bottom: 80px;
  right: 20px;
  width: 320px;
  height: 420px;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.3);
  display: flex;
  flex-direction: column;
  z-index: 1001;
  opacity: 0;
  transform: scale(0.9);
  transition: opacity 0.3s ease, transform 0.3s ease;
}

.chatbotContainer.open {
  opacity: 1;
  transform: scale(1);
}

.chatbotHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);  color: white;
  padding: 10px;
  border-top-left-radius: 8px;
  border-top-right-radius: 8px;
}

.chatbotTitle {
  font-size: 16px;
  font-weight: bold;
}

.closeButton, .clearChatButton {
  background: none;
  border: none;
  color: white;
  font-size: 16px;
  cursor: pointer;
}

.chatbotMessages {
  flex: 1;
  padding: 10px;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  animation: slideIn 0.5s ease;
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.userMessage, .botMessage {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.userMessage {
  justify-content: flex-end;
}

.botMessage {
  justify-content: flex-start;
}

.icon {
  margin: 0 8px;
  font-size: 15px;
}

.messageText {
  max-width: 75%;
  background-color: #f1f1f1;
  padding: 10px;
  font-size: 12px;
  border-radius: 10px;
  color: #333;
  word-wrap: break-word;
  white-space: pre-wrap;
}

.messageText a {
  color: #3f1ec2;
  text-decoration: none;
  font-size: 10px;
  margin-bottom: 5px;
}

.userMessage .messageText {
  background-color: #d1e7ff;
}

.chatbotInput {
  display: flex;
  border-top: 1px solid #ddd;
  padding: 10px;
}

.chatbotInput input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  outline: none;
}

.chatbotInput button {
background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);  color: white;
  border: none;
  padding: 10px;
  border-radius: 4px;
  margin-left: 10px;
  cursor: pointer;
}

.clearChatOverlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1002;
}

.clearChatWindow {
  background: white;
  padding: 20px;
  border-radius: 8px;
  text-align: center;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.3);
}

.confirmButton {
  background-color: #d9534f;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 10px;
}

.cancelButton {
  background-color: #5f1ec1;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}



/* ... previous styles ... */

.botProfile {
  display: flex;
  align-items: center;
}

.botImage {
  width: 35px;
  height: 35px;
  border-radius: 50%;
  margin-right: 10px;
}

.botInfo {
  display: flex;
  flex-direction: column;
}



.botStatus {
  font-size: 12px;
  color: #d1e7ff;
}

/* Rest of the styles */
.headerActions {
  display: flex;
  align-items: center;
}

.closeButton, .clearChatButton {
  background: none;
  border: none;
  color: white;
  font-size: 16px;
  cursor: pointer;
  margin-left: 5px;
}

/* Add this to your existing styles */
.greenDot {
  display: inline-block;
  width: 8px;
  height: 8px;
  background-color: #28a745; /* Green color */
  border-radius: 50%;
  margin-right: 4px;
}

/* Ensure the botStatus class is updated */
.botStatus {
  font-size: 12px;
  color: #d1e7ff;
  display: flex;
  align-items: center;
}

.onlineDot {
  width: 8px;
  height: 8px;
  background-color: #4caf50; /* Green dot */
  border-radius: 50%;
  display: inline-block;
  margin-right: 5px;
}

.minimizeButton {
  background: none;
  border: none;
  color: white;
  font-size: 16px;
  cursor: pointer;
  margin-left: 5px;
}

/* Add tooltip styles */
.clearChatButton[title], .minimizeButton[title] {
  position: relative;
}
