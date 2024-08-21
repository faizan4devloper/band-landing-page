import React, { useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faRobot, faTimes } from '@fortawesome/free-solid-svg-icons';
import { faUserCircle } from '@fortawesome/free-regular-svg-icons';
import styles from './Chatbot.module.css';

const Chatbot = () => {
  const [isOpen, setIsOpen] = useState(false);
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');
  const [loading, setLoading] = useState(false);

  const toggleChatbot = () => {
    setIsOpen(!isOpen);
  };

  const sendMessage = () => {
    if (input.trim() === '') return;

    // Add user message
    const userMessage = { text: input, sender: 'user' };
    setMessages([...messages, userMessage]);
    setInput('');
    setLoading(true);

    // Simulate bot response
    setTimeout(() => {
      const botMessage = { text: 'This is a bot response.', sender: 'bot' };
      setMessages((prevMessages) => [...prevMessages, botMessage]);
      setLoading(false);
    }, 1000);
  };

  return (
    <>
      <div className={styles.chatbotIcon} onClick={toggleChatbot}>
        <FontAwesomeIcon icon={faRobot} />
      </div>
      
      {isOpen && (
        <div className={styles.chatbotContainer}>
          <div className={styles.chatbotHeader}>
            <span>Chatbot</span>
            <button onClick={toggleChatbot} className={styles.closeButton}>
              <FontAwesomeIcon icon={faTimes} />
            </button>
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
                  icon={message.sender === 'user' ? faUserCircle : faRobot}
                  className={styles.icon}
                />
                <div className={styles.messageText}>{message.text}</div>
              </div>
            ))}
            {loading && <div className={styles.loader}>...</div>}
          </div>

          <div className={styles.chatbotInput}>
            <input
              type="text"
              value={input}
              onChange={(e) => setInput(e.target.value)}
              placeholder="Type your message..."
            />
            <button onClick={sendMessage}>
              <FontAwesomeIcon icon={faPaperPlane} />
            </button>
          </div>
        </div>
      )}
    </>
  );
};

export default Chatbot;




/* Keep existing styles */

.loader {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px;
  font-size: 12px;
  color: #5f1ec1;
}

.chatbotInput button {
  background-color: #5f1ec1;
  color: white;
  border: none;
  padding: 10px;
  border-radius: 4px;
  margin-left: 10px;
  cursor: pointer;
  display: flex;
  align-items: center; /* Center align loader and icon */
  justify-content: center; /* Center align loader and icon */
}

.chatbotIcon {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background-color: #5f1ec1;
  color: white;
  padding: 10px;
  border-radius: 50%;
  cursor: pointer;
  z-index: 1000;
}

.chatbotContainer {
  position: fixed;
  bottom: 80px;
  right: 20px;
  width: 300px;
  height: 400px;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.3);
  display: flex;
  flex-direction: column;
  z-index: 1001;
}

.chatbotHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #5f1ec1;
  color: white;
  padding: 10px;
  border-top-left-radius: 8px;
  border-top-right-radius: 8px;
}

.closeButton {
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
  margin: 0 10px;
  font-size: 20px;
}

.messageText {
  max-width: 70%;
  background-color: #f1f1f1;
  padding: 10px;
  border-radius: 10px;
  color: #333;
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
  background-color: #5f1ec1;
  color: white;
  border: none;
  padding: 10px;
  border-radius: 4px;
  margin-left: 10px;
  cursor: pointer;
}

.chatbotInput button:hover {
  background-color: #4a0fa5;
}

.loader {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px;
  font-size: 12px;
  color: #5f1ec1;
}












import React, { useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faTimes } from '@fortawesome/free-solid-svg-icons';
import { BeatLoader } from 'react-spinners';
import styles from './Chatbot.module.css'; // Import your CSS module for styling

const Chatbot = () => {
  const [messages, setMessages] = useState([]);
  const [message, setMessage] = useState('');
  const [isLoading, setIsLoading] = useState(false);
  const [isOpen, setIsOpen] = useState(true);

  const handleSendMessage = async (e) => {
    e.preventDefault();
    if (message.trim()) {
      setIsLoading(true);

      // Add the user's message to the conversation
      setMessages((prevMessages) => [...prevMessages, { type: 'user', text: message }]);

      setMessage('');

      // Simulate bot response delay
      await new Promise((resolve) => setTimeout(resolve, 1000));

      // Add the bot's response to the conversation
      setMessages((prevMessages) => [...prevMessages, { type: 'bot', text: 'This is a bot response.' }]);

      setIsLoading(false);
    }
  };

  const handleClose = () => {
    setIsOpen(false);
  };

  if (!isOpen) return null;

  return (
    <div className={styles.chatbotContainer}>
      <div className={styles.chatbotHeader}>
        <span>Chatbot</span>
        <button className={styles.closeButton} onClick={handleClose}>
          <FontAwesomeIcon icon={faTimes} />
        </button>
      </div>
      <div className={styles.chatbotMessages}>
        {messages.map((msg, index) => (
          <div
            key={index}
            className={`${styles.message} ${msg.type === 'user' ? styles.userMessage : styles.botMessage}`}
          >
            <div className={styles.messageContent}>
              {msg.type === 'user' ? (
                <img src="/path-to-user-icon.png" alt="User" className={styles.userIcon} />
              ) : (
                <img src="/path-to-bot-icon.png" alt="Bot" className={styles.botIcon} />
              )}
              <span>{msg.text}</span>
            </div>
          </div>
        ))}
      </div>
      <div className={styles.chatbotInputContainer}>
        <form onSubmit={handleSendMessage} className={styles.inputForm}>
          <input
            type="text"
            value={message}
            onChange={(e) => setMessage(e.target.value)}
            placeholder="Type a message..."
            className={styles.inputField}
            disabled={isLoading}
          />
          <button type="submit" className={styles.sendButton} disabled={isLoading || !message.trim()}>
            {isLoading ? (
              <div className={styles.loader}>
                <BeatLoader color="#fff" size={8} />
              </div>
            ) : (
              <FontAwesomeIcon icon={faPaperPlane} />
            )}
          </button>
        </form>
      </div>
    </div>
  );
};

export default Chatbot;




.chatbotContainer {
  position: fixed;
  bottom: 20px;
  right: 20px;
  width: 300px;
  background-color: #ffffff;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.chatbotHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  background-color: #5f1ec1;
  color: white;
}

.closeButton {
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
  max-height: 300px; /* Adjust as needed */
}

.message {
  display: flex;
  margin-bottom: 10px;
}

.userMessage {
  justify-content: flex-end;
}

.botMessage {
  justify-content: flex-start;
}

.messageContent {
  display: flex;
  align-items: center;
}

.userIcon, .botIcon {
  width: 24px;
  height: 24px;
  margin-right: 8px;
}

.userMessage .messageContent {
  flex-direction: row-reverse;
  text-align: right;
}

.userMessage .userIcon {
  margin-left: 8px;
  margin-right: 0;
}

.chatbotInputContainer {
  padding: 10px;
  background-color: #f1f1f1;
  border-top: 1px solid #ddd;
}

.inputForm {
  display: flex;
}

.inputField {
  flex: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 20px;
  font-size: 16px;
  outline: none;
}

.sendButton {
  margin-left: 10px;
  padding: 10px;
  background-color: #5f1ec1;
  color: white;
  border: none;
  border-radius: 20px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.sendButton:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.sendButton:hover {
  background-color: #4a0fa5;
}

.loader {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px;
  font-size: 12px;
  color: #fff;
}











import React, { useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faRobot, faTimes } from '@fortawesome/free-solid-svg-icons';
import { faUserCircle } from '@fortawesome/free-regular-svg-icons';
import styles from './Chatbot.module.css';

const Chatbot = () => {
  const [isOpen, setIsOpen] = useState(false);
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');
  const [loading, setLoading] = useState(false);

  const toggleChatbot = () => {
    setIsOpen(!isOpen);
  };

  const sendMessage = () => {
    if (input.trim() === '') return;

    // Add user message
    const userMessage = { text: input, sender: 'user' };
    setMessages([...messages, userMessage]);
    setInput('');
    setLoading(true);

    // Simulate bot response
    setTimeout(() => {
      const botMessage = { text: 'This is a bot response.', sender: 'bot' };
      setMessages((prevMessages) => [...prevMessages, botMessage]);
      setLoading(false);
    }, 1000);
  };

  return (
    <>
      <div className={styles.chatbotIcon} onClick={toggleChatbot}>
        <FontAwesomeIcon icon={faRobot} />
      </div>
      
      {isOpen && (
        <div className={styles.chatbotContainer}>
          <div className={styles.chatbotHeader}>
            <span>Chatbot</span>
            <button onClick={toggleChatbot} className={styles.closeButton}>
              <FontAwesomeIcon icon={faTimes} />
            </button>
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
                  icon={message.sender === 'user' ? faUserCircle : faRobot}
                  className={styles.icon}
                />
                <div className={styles.messageText}>{message.text}</div>
              </div>
            ))}
            {loading && <div className={styles.loader}>...</div>}
          </div>

          <div className={styles.chatbotInput}>
            <input
              type="text"
              value={input}
              onChange={(e) => setInput(e.target.value)}
              placeholder="Type your message..."
            />
            <button onClick={sendMessage}>
              <FontAwesomeIcon icon={faPaperPlane} />
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
  background-color: #5f1ec1;
  color: white;
  padding: 10px;
  border-radius: 50%;
  cursor: pointer;
  z-index: 1000;
}

.chatbotContainer {
  position: fixed;
  bottom: 80px;
  right: 20px;
  width: 300px;
  height: 400px;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.3);
  display: flex;
  flex-direction: column;
  z-index: 1001;
}

.chatbotHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #5f1ec1;
  color: white;
  padding: 10px;
  border-top-left-radius: 8px;
  border-top-right-radius: 8px;
}

.closeButton {
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
  margin: 0 10px;
  font-size: 20px;
}

.messageText {
  max-width: 70%;
  background-color: #f1f1f1;
  padding: 10px;
  border-radius: 10px;
  color: #333;
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
  background-color: #5f1ec1;
  color: white;
  border: none;
  padding: 10px;
  border-radius: 4px;
  margin-left: 10px;
  cursor: pointer;
}

.chatbotInput button:hover {
  background-color: #4a0fa5;
}

.loader {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px;
  font-size: 12px;
  color: #5f1ec1;
}











import React, { useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faTimes } from '@fortawesome/free-solid-svg-icons';
import { BeatLoader } from 'react-spinners';
import styles from './Chatbot.module.css'; // Import your CSS module for styling

const Chatbot = () => {
  const [messages, setMessages] = useState([]);
  const [message, setMessage] = useState('');
  const [isLoading, setIsLoading] = useState(false);
  const [isOpen, setIsOpen] = useState(true);

  const handleSendMessage = async (e) => {
    e.preventDefault();
    if (message.trim()) {
      setIsLoading(true);

      // Add the user's message to the conversation
      setMessages((prevMessages) => [...prevMessages, { type: 'user', text: message }]);

      setMessage('');

      // Simulate bot response delay
      await new Promise((resolve) => setTimeout(resolve, 1000));

      // Add the bot's response to the conversation
      setMessages((prevMessages) => [...prevMessages, { type: 'bot', text: 'This is a bot response.' }]);

      setIsLoading(false);
    }
  };

  const handleClose = () => {
    setIsOpen(false);
  };

  if (!isOpen) return null;

  return (
    <div className={styles.chatbotContainer}>
      <div className={styles.chatbotHeader}>
        <span>Chatbot</span>
        <button className={styles.closeButton} onClick={handleClose}>
          <FontAwesomeIcon icon={faTimes} />
        </button>
      </div>
      <div className={styles.chatbotMessages}>
        {messages.map((msg, index) => (
          <div
            key={index}
            className={`${styles.message} ${msg.type === 'user' ? styles.userMessage : styles.botMessage}`}
          >
            <div className={styles.messageContent}>
              {msg.type === 'user' ? (
                <img src="/path-to-user-icon.png" alt="User" className={styles.userIcon} />
              ) : (
                <img src="/path-to-bot-icon.png" alt="Bot" className={styles.botIcon} />
              )}
              <span>{msg.text}</span>
            </div>
          </div>
        ))}
      </div>
      <div className={styles.chatbotInputContainer}>
        <form onSubmit={handleSendMessage} className={styles.inputForm}>
          <input
            type="text"
            value={message}
            onChange={(e) => setMessage(e.target.value)}
            placeholder="Type a message..."
            className={styles.inputField}
            disabled={isLoading}
          />
          <button type="submit" className={styles.sendButton} disabled={isLoading || !message.trim()}>
            {isLoading ? <BeatLoader color="#fff" size={8} /> : <FontAwesomeIcon icon={faPaperPlane} />}
          </button>
        </form>
      </div>
    </div>
  );
};

export default Chatbot;



.chatbotContainer {
  position: fixed;
  bottom: 20px;
  right: 20px;
  width: 300px;
  background-color: #ffffff;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.chatbotHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  background-color: #5f1ec1;
  color: white;
}

.closeButton {
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
  max-height: 300px; /* Adjust as needed */
}

.message {
  display: flex;
  margin-bottom: 10px;
}

.userMessage {
  justify-content: flex-end;
}

.botMessage {
  justify-content: flex-start;
}

.messageContent {
  display: flex;
  align-items: center;
}

.userIcon, .botIcon {
  width: 24px;
  height: 24px;
  margin-right: 8px;
}

.userMessage .messageContent {
  flex-direction: row-reverse;
  text-align: right;
}

.userMessage .userIcon {
  margin-left: 8px;
  margin-right: 0;
}

.chatbotInputContainer {
  padding: 10px;
  background-color: #f1f1f1;
  border-top: 1px solid #ddd;
}

.inputForm {
  display: flex;
}

.inputField {
  flex: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 20px;
  font-size: 16px;
  outline: none;
}

.sendButton {
  margin-left: 10px;
  padding: 10px;
  background-color: #5f1ec1;
  color: white;
  border: none;
  border-radius: 20px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.sendButton:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}






import React, { useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane } from '@fortawesome/free-solid-svg-icons';
import { BeatLoader } from 'react-spinners';
import styles from './Chatbot.module.css';

const Chatbot = () => {
  const [messages, setMessages] = useState([]);
  const [message, setMessage] = useState('');
  const [isLoading, setIsLoading] = useState(false);

  const handleSendMessage = async (e) => {
    e.preventDefault();
    if (message.trim()) {
      setIsLoading(true);
      // Add user message
      setMessages([...messages, { type: 'user', text: message }]);
      setMessage('');

      // Simulate sending a message
      await new Promise((resolve) => setTimeout(resolve, 1000));

      // Add bot response
      setMessages((prevMessages) => [
        ...prevMessages,
        { type: 'bot', text: 'This is a bot response.' },
      ]);
      setIsLoading(false);
    }
  };

  return (
    <div className={styles.chatbot}>
      <div className={styles.chatHeader}>
        <h2>Chatbot</h2>
        <button className={styles.closeButton}>X</button>
      </div>

      <div className={styles.chatWindow}>
        {messages.map((msg, index) => (
          <div
            key={index}
            className={
              msg.type === 'user'
                ? `${styles.message} ${styles.userMessage}`
                : `${styles.message} ${styles.botMessage}`
            }
          >
            <div className={styles.icon}>
              <img
                src={msg.type === 'user' ? '/path/to/user-icon.png' : '/path/to/bot-icon.png'}
                alt={msg.type === 'user' ? 'User Icon' : 'Bot Icon'}
              />
            </div>
            <div className={styles.text}>{msg.text}</div>
          </div>
        ))}
      </div>

      <div className={styles.inputSection}>
        <form onSubmit={handleSendMessage} className={styles.inputForm}>
          <input
            type="text"
            value={message}
            onChange={(e) => setMessage(e.target.value)}
            placeholder="Type a message..."
            className={styles.inputField}
            disabled={isLoading}
          />
          <button type="submit" className={styles.sendButton} disabled={isLoading}>
            {isLoading ? (
              <BeatLoader color="#fff" size={8} />
            ) : (
              <FontAwesomeIcon icon={faPaperPlane} />
            )}
          </button>
        </form>
      </div>
    </div>
  );
};

export default Chatbot;


.chatbot {
  width: 400px;
  border: 1px solid #ddd;
  border-radius: 10px;
  background-color: #fff;
  display: flex;
  flex-direction: column;
}

.chatHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  background-color: #5f1ec1;
  color: #fff;
  border-bottom: 1px solid #ddd;
}

.closeButton {
  background: none;
  border: none;
  color: #fff;
  font-size: 20px;
  cursor: pointer;
}

.chatWindow {
  flex: 1;
  padding: 10px;
  overflow-y: auto;
}

.message {
  display: flex;
  align-items: flex-start;
  margin-bottom: 10px;
}

.userMessage {
  justify-content: flex-end;
  text-align: right;
}

.botMessage {
  justify-content: flex-start;
  text-align: left;
}

.icon {
  width: 30px;
  height: 30px;
  margin-right: 10px;
}

.text {
  max-width: 70%;
  padding: 10px;
  background-color: #f1f1f1;
  border-radius: 20px;
}

.userMessage .text {
  background-color: #5f1ec1;
  color: #fff;
}

.inputSection {
  padding: 10px;
  border-top: 1px solid #ddd;
  background-color: #f1f1f1;
}

.inputForm {
  display: flex;
  align-items: center;
}

.inputField {
  flex: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 20px;
  font-size: 16px;
  outline: none;
}

.sendButton {
  margin-left: 10px;
  padding: 10px;
  background-color: #5f1ec1;
  color: white;
  border: none;
  border-radius: 20px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.sendButton:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}





import React, { useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane } from '@fortawesome/free-solid-svg-icons';
import { BeatLoader } from 'react-spinners';
import styles from './ChatbotInput.module.css';

const ChatbotInput = ({ onSendMessage }) => {
  const [message, setMessage] = useState('');
  const [isLoading, setIsLoading] = useState(false);

  const handleSubmit = async (e) => {
    e.preventDefault();
    if (message.trim()) {
      setIsLoading(true);
      await onSendMessage(message);
      setMessage('');
      setIsLoading(false);
    }
  };

  return (
    <div className={styles.chatbotInput}>
      <form onSubmit={handleSubmit} className={styles.inputForm}>
        <input
          type="text"
          value={message}
          onChange={(e) => setMessage(e.target.value)}
          placeholder="Type a message..."
          className={styles.inputField}
          disabled={isLoading}
        />
        <button type="submit" className={styles.sendButton} disabled={isLoading}>
          {isLoading ? (
            <BeatLoader color="#fff" size={8} />
          ) : (
            <FontAwesomeIcon icon={faPaperPlane} />
          )}
        </button>
      </form>
    </div>
  );
};

export default ChatbotInput;


.chatbotInput {
  display: flex;
  align-items: center;
  padding: 10px;
  background-color: #f1f1f1;
  border-top: 1px solid #ddd;
}

.inputForm {
  display: flex;
  width: 100%;
}

.inputField {
  flex: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 20px;
  font-size: 16px;
  outline: none;
}

.sendButton {
  margin-left: 10px;
  padding: 10px;
  background-color: #5f1ec1;
  color: white;
  border: none;
  border-radius: 20px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.sendButton:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}


import React, { useState } from 'react';
import ChatbotInput from './ChatbotInput';
// Other imports...

const Chatbot = () => {
  const [messages, setMessages] = useState([]);

  const handleSendMessage = async (message) => {
    // Simulate a delay to represent sending message
    await new Promise((resolve) => setTimeout(resolve, 1000));
    
    // Add the user message
    setMessages([...messages, { type: 'user', text: message }]);

    // Add a bot response after a delay
    await new Promise((resolve) => setTimeout(resolve, 1000));
    setMessages((prevMessages) => [...prevMessages, { type: 'bot', text: 'This is a bot response.' }]);
  };

  return (
    <div className="chatbot">
      {/* Other parts of the Chatbot */}
      <ChatbotInput onSendMessage={handleSendMessage} />
    </div>
  );
};

export default Chatbot;









import React, { useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faRobot, faTimes } from '@fortawesome/free-solid-svg-icons';
import { faUserCircle } from '@fortawesome/free-regular-svg-icons';
import styles from './Chatbot.module.css';

const Chatbot = () => {
  const [isOpen, setIsOpen] = useState(false);
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');
  const [loading, setLoading] = useState(false);

  const toggleChatbot = () => {
    setIsOpen(!isOpen);
  };

  const sendMessage = () => {
    if (input.trim() === '') return;

    // Add user message
    const userMessage = { text: input, sender: 'user' };
    setMessages([...messages, userMessage]);
    setInput('');
    setLoading(true);

    // Simulate bot response
    setTimeout(() => {
      const botMessage = { text: 'This is a bot response.', sender: 'bot' };
      setMessages((prevMessages) => [...prevMessages, botMessage]);
      setLoading(false);
    }, 1000);
  };

  return (
    <>
      <div className={styles.chatbotIcon} onClick={toggleChatbot}>
        <FontAwesomeIcon icon={faRobot} />
      </div>
      
      {isOpen && (
        <div className={styles.chatbotContainer}>
          <div className={styles.chatbotHeader}>
            <span>Chatbot</span>
            <button onClick={toggleChatbot} className={styles.closeButton}>
              <FontAwesomeIcon icon={faTimes} />
            </button>
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
                  icon={message.sender === 'user' ? faUserCircle : faRobot}
                  className={styles.icon}
                />
                <div className={styles.messageText}>{message.text}</div>
              </div>
            ))}
            {loading && <div className={styles.loader}>...</div>}
          </div>

          <div className={styles.chatbotInput}>
            <input
              type="text"
              value={input}
              onChange={(e) => setInput(e.target.value)}
              placeholder="Type your message..."
            />
            <button onClick={sendMessage}>
              <FontAwesomeIcon icon={faPaperPlane} />
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
  background-color: #5f1ec1;
  color: white;
  padding: 10px;
  border-radius: 50%;
  cursor: pointer;
  z-index: 1000;
}

.chatbotContainer {
  position: fixed;
  bottom: 80px;
  right: 20px;
  width: 300px;
  height: 400px;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.3);
  display: flex;
  flex-direction: column;
  z-index: 1001;
}

.chatbotHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #5f1ec1;
  color: white;
  padding: 10px;
  border-top-left-radius: 8px;
  border-top-right-radius: 8px;
}

.closeButton {
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
  margin: 0 10px;
  font-size: 20px;
}

.messageText {
  max-width: 70%;
  background-color: #f1f1f1;
  padding: 10px;
  border-radius: 10px;
  color: #333;
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
  background-color: #5f1ec1;
  color: white;
  border: none;
  padding: 10px;
  border-radius: 4px;
  margin-left: 10px;
  cursor: pointer;
}

.chatbotInput button:hover {
  background-color: #4a0fa5;
}

.loader {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px;
  font-size: 12px;
  color: #5f1ec1;
}






import React, { useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faUser, faRobot } from '@fortawesome/free-solid-svg-icons';
import BeatLoader from 'react-spinners/BeatLoader';
import styles from './Chatbot.module.css';

const Chatbot = () => {
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');
  const [loading, setLoading] = useState(false);

  const handleSend = () => {
    if (input.trim()) {
      const userMessage = { text: input, sender: 'user' };
      setMessages([...messages, userMessage]);
      setInput('');
      setLoading(true);

      // Simulate bot response after a delay
      setTimeout(() => {
        const botMessage = { text: 'This is a response from the bot.', sender: 'bot' };
        setMessages(prevMessages => [...prevMessages, botMessage]);
        setLoading(false);
      }, 1000);
    }
  };

  return (
    <div className={styles.chatbotContainer}>
      <div className={styles.chatbotIcon}>Chat</div>
      <div className={styles.chatWindow}>
        <div className={styles.messagesContainer}>
          {messages.map((message, index) => (
            <div
              key={index}
              className={`${styles.message} ${
                message.sender === 'user' ? styles.userMessage : styles.botMessage
              }`}
            >
              <div className={styles.iconContainer}>
                <FontAwesomeIcon
                  icon={message.sender === 'user' ? faUser : faRobot}
                  className={styles.icon}
                />
              </div>
              <div className={styles.textContainer}>{message.text}</div>
            </div>
          ))}
          {loading && (
            <div className={`${styles.message} ${styles.botMessage}`}>
              <div className={styles.iconContainer}>
                <FontAwesomeIcon icon={faRobot} className={styles.icon} />
              </div>
              <div className={styles.loaderContainer}>
                <BeatLoader color="#5931d5" loading={loading} size={10} margin={2} />
              </div>
            </div>
          )}
        </div>
        <div className={styles.inputContainer}>
          <input
            type="text"
            className={styles.chatInput}
            value={input}
            onChange={e => setInput(e.target.value)}
            placeholder="Type your message..."
          />
          <button className={styles.sendButton} onClick={handleSend}>
            <FontAwesomeIcon icon={faPaperPlane} />
          </button>
        </div>
      </div>
    </div>
  );
};

export default Chatbot;



.chatbotContainer {
  position: fixed;
  bottom: 20px;
  right: 20px;
  width: 300px;
  background-color: white;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  border-radius: 8px;
  overflow: hidden;
}

.chatbotIcon {
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  color: white;
  padding: 10px;
  text-align: center;
  cursor: pointer;
}

.chatWindow {
  display: flex;
  flex-direction: column;
  height: 400px;
}

.messagesContainer {
  flex: 1;
  padding: 10px;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
}

.message {
  display: flex;
  align-items: flex-end;
  margin-bottom: 10px;
}

.userMessage {
  flex-direction: row-reverse;
  text-align: right;
}

.botMessage {
  flex-direction: row;
  text-align: left;
}

.iconContainer {
  width: 30px;
  height: 30px;
  margin: 0 10px;
}

.icon {
  font-size: 24px;
  color: #5f1ec1;
}

.textContainer {
  max-width: 70%;
  padding: 10px;
  border-radius: 12px;
  background-color: #f1f1f1;
}

.userMessage .textContainer {
  background-color: #5f1ec1;
  color: white;
}

.inputContainer {
  display: flex;
  padding: 10px;
  border-top: 1px solid #ccc;
}

.chatInput {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.sendButton {
  background-color: #5f1ec1;
  color: white;
  padding: 10px;
  border: none;
  border-radius: 4px;
  margin-left: 10px;
  cursor: pointer;
}

.loaderContainer {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 5px;
  border-radius: 12px;
  background-color: #f1f1f1;
}






import React, { useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faComments, faPaperPlane, faSpinner } from '@fortawesome/free-solid-svg-icons';
import styles from './Chatbot.module.css';

const Chatbot = () => {
  const [isOpen, setIsOpen] = useState(false);
  const [messages, setMessages] = useState([]);
  const [inputValue, setInputValue] = useState('');
  const [loading, setLoading] = useState(false);

  const toggleChatbot = () => {
    setIsOpen(!isOpen);
  };

  const handleSend = () => {
    if (inputValue.trim() === '') return;

    const newMessage = { text: inputValue, sender: 'user' };
    setMessages([...messages, newMessage]);
    setInputValue('');
    setLoading(true);

    setTimeout(() => {
      const botResponse = { text: 'This is a bot response.', sender: 'bot' };
      setMessages((prevMessages) => [...prevMessages, botResponse]);
      setLoading(false);
    }, 1500); // Simulate bot response delay
  };

  return (
    <>
      <div className={styles.chatbotIcon} onClick={toggleChatbot}>
        <FontAwesomeIcon icon={faComments} />
      </div>
      {isOpen && (
        <div className={styles.chatbotWindow}>
          <div className={styles.chatHeader}>Chatbot</div>
          <div className={styles.chatBody}>
            {messages.map((message, index) => (
              <div
                key={index}
                className={`${styles.message} ${message.sender === 'user' ? styles.userMessage : styles.botMessage}`}
              >
                <div className={styles.icon}>
                  {message.sender === 'user' ? '👤' : '🤖'}
                </div>
                <div className={styles.messageText}>{message.text}</div>
              </div>
            ))}
            {loading && (
              <div className={styles.loader}>
                <FontAwesomeIcon icon={faSpinner} spin />
              </div>
            )}
          </div>
          <div className={styles.chatInputArea}>
            <input
              type="text"
              value={inputValue}
              onChange={(e) => setInputValue(e.target.value)}
              className={styles.chatInput}
              placeholder="Type your message..."
            />
            <button className={styles.sendButton} onClick={handleSend}>
              <FontAwesomeIcon icon={faPaperPlane} />
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
  background-color: #5931d5;
  color: white;
  border-radius: 50%;
  width: 50px;
  height: 50px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.chatbotIcon:hover {
  background-color: #4327a8;
}

.chatbotWindow {
  position: fixed;
  bottom: 80px;
  right: 20px;
  width: 300px;
  max-height: 400px;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0px 4px 15px rgba(0, 0, 0, 0.2);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.chatHeader {
  padding: 10px;
  background-color: #5931d5;
  color: white;
  text-align: center;
  border-top-left-radius: 8px;
  border-top-right-radius: 8px;
  font-weight: bold;
}

.chatBody {
  padding: 10px;
  overflow-y: auto;
  flex-grow: 1;
}

.message {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.icon {
  margin-right: 10px;
  font-size: 24px;
}

.messageText {
  background-color: #f1f1f1;
  padding: 10px;
  border-radius: 8px;
  max-width: 70%;
  word-wrap: break-word;
}

.userMessage .messageText {
  background-color: #d4e6ff;
  align-self: flex-end;
}

.botMessage .messageText {
  background-color: #e9e9e9;
}

.chatInputArea {
  display: flex;
  align-items: center;
  padding: 10px;
  border-top: 1px solid #ccc;
}

.chatInput {
  flex-grow: 1;
  border: 1px solid #ccc;
  border-radius: 20px;
  padding: 10px;
  font-size: 14px;
  outline: none;
}

.sendButton {
  background-color: #5931d5;
  color: white;
  border: none;
  padding: 10px 15px;
  margin-left: 10px;
  border-radius: 50%;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.sendButton:hover {
  background-color: #4327a8;
}

.loader {
  display: flex;
  justify-content: center;
  margin: 10px 0;
}









import React, { useState, useEffect, useRef } from 'react';
import { BrowserRouter as Router, Routes, Route, useLocation } from 'react-router-dom';
import Header from './components/Header/Header';
import Home from './Home';
import SideBarPage from './components/Sidebar/SideBarPage';
import AllCardsPage from './components/Cards/AllCardsPage';
import ChatBot from './components/ChatBot/ChatBot';
import { getCardsData } from './data';
import { BeatLoader } from 'react-spinners';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';
import styles from './App.module.css';

const MainApp = () => {
  const [loading, setLoading] = useState(true);
  const [cardsData, setCardsData] = useState([]);
  const [currentIndex, setCurrentIndex] = useState(0);
  const [bigIndex, setBigIndex] = useState(null);
  const [showScrollDown, setShowScrollDown] = useState(false);
  const [showScrollUp, setShowScrollUp] = useState(false);
  const cardsContainerRef = useRef(null);
  const location = useLocation();

  useEffect(() => {
    const fetchData = async () => {
      const data = await getCardsData();
      setCardsData(data);
      setLoading(false);
    };

    fetchData();
  }, []);

  const toggleSize = (index) => {
    setBigIndex(index === bigIndex ? null : index);
  };

  const handleScrollDown = () => {
    if (cardsContainerRef.current) {
      cardsContainerRef.current.scrollBy({ top: 500, behavior: 'smooth' });
    }
  };

  const handleScrollUp = () => {
    if (cardsContainerRef.current) {
      cardsContainerRef.current.scrollBy({ top: -500, behavior: 'smooth' });
    }
  };

  useEffect(() => {
    const handleScroll = () => {
      if (cardsContainerRef.current) {
        setShowScrollDown(cardsContainerRef.current.scrollTop + cardsContainerRef.current.clientHeight < cardsContainerRef.current.scrollHeight);
        setShowScrollUp(cardsContainerRef.current.scrollTop > 0);
      }
    };

    if (cardsContainerRef.current) {
      cardsContainerRef.current.addEventListener('scroll', handleScroll);
      return () => {
        cardsContainerRef.current.removeEventListener('scroll', handleScroll);
      };
    }
  }, [cardsContainerRef]);

  const handleClickLeft = () => {
    setCurrentIndex((prevIndex) => (prevIndex === 0 ? cardsData.length - 1 : prevIndex - 1));
  };

  const handleClickRight = () => {
    setCurrentIndex((prevIndex) => (prevIndex === cardsData.length - 1 ? 0 : prevIndex + 1));
  };

  return (
    <div className={styles.app}>
      {loading ? (
        <div className={styles.loader}>
          <BeatLoader color="#5931d5" loading={loading} size={15} margin={2} />
        </div>
      ) : (
        <>
          <Header />
          <Routes>
            <Route
              path="/"
              element={
                <Home
                  cardsData={cardsData}
                  handleClickLeft={handleClickLeft}
                  handleClickRight={handleClickRight}
                  currentIndex={currentIndex}
                  bigIndex={bigIndex}
                  toggleSize={toggleSize}
                  cardsContainerRef={cardsContainerRef}
                />
              }
            />
            <Route path="/dashboard" element={<SideBarPage />} />
            <Route
              path="/all-cards"
              element={<AllCardsPage cardsData={cardsData} cardsContainerRef={cardsContainerRef} />}
            />
          </Routes>
          {showScrollDown && location.pathname !== '/all-cards' && location.pathname !== '/dashboard' && (
            <div className={styles.scrollDownButton} onClick={handleScrollDown} title="Scroll Down">
              <FontAwesomeIcon icon={faChevronDown} />
            </div>
          )}
          {showScrollUp && (
            <div className={styles.scrollUpButton} onClick={handleScrollUp} title="Scroll Up">
              <FontAwesomeIcon icon={faChevronUp} />
            </div>
          )}
          <ChatBot />
        </>
      )}
    </div>
  );
};

const App = () => (
  <Router>
    <MainApp />
  </Router>
);

export default App;




// src/components/ChatBot/ChatBot.js
import React, { useState } from 'react';
import styles from './ChatBot.module.css';
import userIcon from './icons/userIcon.png'; // Add your user icon path
import botIcon from './icons/botIcon.png'; // Add your bot icon path

const ChatBot = () => {
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');
  const [loading, setLoading] = useState(false);
  const [chatVisible, setChatVisible] = useState(false);

  const handleSend = () => {
    if (input.trim()) {
      setMessages([...messages, { text: input, sender: 'user' }]);
      setInput('');
      setLoading(true);

      // Simulate bot response
      setTimeout(() => {
        setMessages([...messages, { text: input, sender: 'user' }, { text: 'Bot response...', sender: 'bot' }]);
        setLoading(false);
      }, 1000);
    }
  };

  return (
    <div className={`${styles.chatBotContainer} ${chatVisible ? styles.visible : styles.hidden}`}>
      <div className={styles.chatHeader} onClick={() => setChatVisible(!chatVisible)}>
        <span>ChatBot</span>
      </div>
      <div className={styles.chatBody}>
        {messages.map((message, index) => (
          <div key={index} className={`${styles.message} ${message.sender === 'user' ? styles.userMessage : styles.botMessage}`}>
            <img src={message.sender === 'user' ? userIcon : botIcon} alt={message.sender} className={styles.messageIcon} />
            <span>{message.text}</span>
          </div>
        ))}
        {loading && <div className={styles.loader}>...</div>}
      </div>
      <div className={styles.chatFooter}>
        <input
          type="text"
          value={input}
          onChange={(e) => setInput(e.target.value)}
          placeholder="Type a message..."
        />
        <button onClick={handleSend}>Send</button>
      </div>
    </div>
  );
};

export default ChatBot;



/* src/components/ChatBot/ChatBot.module.css */
.chatBotContainer {
  position: fixed;
  right: 20px;
  bottom: 20px;
  width: 300px;
  max-height: 500px;
  border: 1px solid #ccc;
  border-radius: 10px;
  background-color: #fff;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

.chatHeader {
  background-color: #5f1ec1;
  color: white;
  padding: 10px;
  cursor: pointer;
  text-align: center;
}

.chatBody {
  flex: 1;
  padding: 10px;
  overflow-y: auto;
}

.message {
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

.messageIcon {
  width: 24px;
  height: 24px;
  margin-right: 8px;
}

.chatFooter {
  display: flex;
  padding: 10px;
  background-color: #f1f1f1;
}

.chatFooter input {
  flex: 1;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.chatFooter button {
  padding: 8px 16px;
  margin-left: 8px;
  border: none;
  border-radius: 4px;
  background-color: #5f1ec1;
  color: white;
  cursor: pointer;
}

.chatFooter button:hover {
  background-color: #4a00e0;
}

.hidden {
  display: none;
}

.visible {
  display: flex;
  flex-direction: column;
}

.loader {
  margin: 20px;
  text-align: center;
}


import React from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import Header from './components/Header/Header';
import Home from './Home';
import SideBarPage from './components/Sidebar/SideBarPage';
import AllCardsPage from './components/Cards/AllCardsPage';
import ChatBot from './components/ChatBot/ChatBot';
import { getCardsData } from './data';
import { BeatLoader } from 'react-spinners';
import styles from './App.module.css';

const MainApp = () => {
  // Existing code ...

  return (
    <div className={styles.app}>
      {loading ? (
        <div className={styles.loader}>
          <BeatLoader color="#5931d5" loading={loading} size={15} margin={2} />
        </div>
      ) : (
        <>
          <Header />
          <Routes>
            <Route
              path="/"
              element={
                <Home
                  cardsData={cardsData}
                  handleClickLeft={handleClickLeft}
                  handleClickRight={handleClickRight}
                  currentIndex={currentIndex}
                  bigIndex={bigIndex}
                  toggleSize={toggleSize}
                  cardsContainerRef={cardsContainerRef}
                />
              }
            />
            <Route path="/dashboard" element={<SideBarPage />} />
            <Route
              path="/all-cards"
              element={<AllCardsPage cardsData={cardsData} cardsContainerRef={cardsContainerRef} />}
            />
          </Routes>
          {showScrollDown && location.pathname !== '/all-cards' && location.pathname !== '/dashboard' && (
            <div className={styles.scrollDownButton} onClick={handleScrollDown} title="Scroll Down">
              <FontAwesomeIcon icon={faChevronDown} />
            </div>
          )}
          {showScrollUp && (
            <div className={styles.scrollUpButton} onClick={handleScrollUp} title="Scroll Up">
              <FontAwesomeIcon icon={faChevronUp} />
            </div>
          )}
          <ChatBot />
        </>
      )}
    </div>
  );
};

const App = () => (
  <Router>
    <MainApp />
  </Router>
);

export default App;









import React, { useState, useEffect, useRef } from 'react';
import { BrowserRouter as Router, Routes, Route, useLocation } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp, faComments } from '@fortawesome/free-solid-svg-icons'; // Import the chatbot icon
import Header from './components/Header/Header';
import Home from './Home';
import SideBarPage from './components/Sidebar/SideBarPage';
import AllCardsPage from './components/Cards/AllCardsPage';
import { getCardsData } from './data';
import { BeatLoader } from 'react-spinners';
import styles from './App.module.css';

const MainApp = () => {
  const location = useLocation();
  const [cardsData, setCardsData] = useState([]);
  const [currentIndex, setCurrentIndex] = useState(0);
  const [bigIndex, setBigIndex] = useState(null);
  const [showScrollDown, setShowScrollDown] = useState(true);
  const [showScrollUp, setShowScrollUp] = useState(false);
  const [loading, setLoading] = useState(true);
  const [chatbotOpen, setChatbotOpen] = useState(false); // State to track the chatbot window
  const cardsContainerRef = useRef(null);

  useEffect(() => {
    const fetchData = async () => {
      const data = await getCardsData();
      setCardsData(data);
      setLoading(false);
    };

    fetchData();
  }, []);

  const toggleSize = (index) => {
    setBigIndex(index === bigIndex ? null : index);
  };

  const handleClickLeft = () => {
    const newBigIndex = bigIndex === null || bigIndex === 0 ? cardsData.length - 1 : bigIndex - 1;
    setBigIndex(newBigIndex);
    const newCurrentIndex = newBigIndex < currentIndex ? newBigIndex : currentIndex;
    setCurrentIndex(newCurrentIndex);
  };

  const handleClickRight = () => {
    const newBigIndex = bigIndex === null || bigIndex === cardsData.length - 1 ? 0 : bigIndex + 1;
    setBigIndex(newBigIndex);
    const newCurrentIndex = newBigIndex > currentIndex + 4 ? newBigIndex - 4 : currentIndex;
    setCurrentIndex(newCurrentIndex);
  };

  const handleScrollDown = () => {
    if (cardsContainerRef.current) {
      cardsContainerRef.current.scrollIntoView({ behavior: 'smooth' });
      setShowScrollDown(false);
      setShowScrollUp(true);
    }
  };

  const handleScrollUp = () => {
    window.scrollTo({ top: 0, behavior: 'smooth' });
    setShowScrollDown(true);
    setShowScrollUp(false);
  };

  useEffect(() => {
    const handleScroll = () => {
      if (window.scrollY > 50) {
        setShowScrollUp(true);
        setShowScrollDown(false);
      } else {
        setShowScrollUp(false);
        setShowScrollDown(true);
      }
    };

    const debounceScroll = debounce(handleScroll, 100);
    window.addEventListener('scroll', debounceScroll);

    return () => window.removeEventListener('scroll', debounceScroll);
  }, []);

  const debounce = (func, wait) => {
    let timeout;
    return function executedFunction(...args) {
      const later = () => {
        clearTimeout(timeout);
        func(...args);
      };
      clearTimeout(timeout);
      timeout = setTimeout(later, wait);
    };
  };

  const toggleChatbot = () => {
    setChatbotOpen(!chatbotOpen); // Toggle the chatbot window
  };

  return (
    <div className={styles.app}>
      {loading ? (
        <div className={styles.loader}>
          <BeatLoader color="#5931d5" loading={loading} size={15} margin={2} />
        </div>
      ) : (
        <>
          <Header />
          <Routes>
            <Route
              path="/"
              element={
                <Home
                  cardsData={cardsData}
                  handleClickLeft={handleClickLeft}
                  handleClickRight={handleClickRight}
                  currentIndex={currentIndex}
                  bigIndex={bigIndex}
                  toggleSize={toggleSize}
                  cardsContainerRef={cardsContainerRef}
                />
              }
            />
            <Route path="/dashboard" element={<SideBarPage />} />
            <Route
              path="/all-cards"
              element={<AllCardsPage cardsData={cardsData} cardsContainerRef={cardsContainerRef} />}
            />
          </Routes>
          {showScrollDown && location.pathname !== '/all-cards' && location.pathname !== '/dashboard' && (
            <div className={styles.scrollDownButton} onClick={handleScrollDown} title="Scroll Down">
              <FontAwesomeIcon icon={faChevronDown} />
            </div>
          )}
          {showScrollUp && (
            <div className={styles.scrollUpButton} onClick={handleScrollUp} title="Scroll Up">
              <FontAwesomeIcon icon={faChevronUp} />
            </div>
          )}
          {/* Chatbot Icon */}
          <div className={styles.chatbotIcon} onClick={toggleChatbot} title="Chat with us">
            <FontAwesomeIcon icon={faComments} />
          </div>
          {/* Chatbot Window */}
          {chatbotOpen && (
            <div className={styles.chatbotWindow}>
              {/* Your chatbot component goes here */}
              <div className={styles.chatbotHeader}>Chatbot</div>
              <div className={styles.chatbotContent}>
                {/* Chatbot content */}
              </div>
            </div>
          )}
        </>
      )}
    </div>
  );
};

const App = () => (
  <Router>
    <MainApp />
  </Router>
);

export default App;


.chatbotIcon {
  position: fixed;
  right: 20px;
  bottom: 20px;
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  color: white;
  border: none;
  border-radius: 50%;
  width: 50px;
  height: 50px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 20px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease;
  z-index: 1001; /* Ensure it stays on top */
}

.chatbotIcon:hover {
  background-color: rgba(13, 85, 198, 1);
}

.chatbotWindow {
  position: fixed;
  right: 20px;
  bottom: 80px;
  width: 350px;
  height: 400px;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
  z-index: 1000; /* Ensure it stays on top */
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.chatbotHeader {
  background-color: #6f36cd;
  color: white;
  padding: 10px;
  text-align: center;
  font-weight: bold;
}

.chatbotContent {
  flex: 1;
  padding: 10px;
  overflow-y: auto;
}









import React, { useState, useEffect, useRef } from 'react';
import { BrowserRouter as Router, Routes, Route, useLocation } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';
import Header from './components/Header/Header';
import Home from './Home';
import SideBarPage from './components/Sidebar/SideBarPage';
import AllCardsPage from './components/Cards/AllCardsPage';
import { getCardsData } from './data';
import { BeatLoader } from 'react-spinners';
import styles from './App.module.css';

const MainApp = () => {
  const location = useLocation();
  const [cardsData, setCardsData] = useState([]);
  const [currentIndex, setCurrentIndex] = useState(0);
  const [bigIndex, setBigIndex] = useState(null);
  const [showScrollDown, setShowScrollDown] = useState(true);
  const [showScrollUp, setShowScrollUp] = useState(false);
  const [loading, setLoading] = useState(true);
  const cardsContainerRef = useRef(null);

  useEffect(() => {
    const fetchData = async () => {
      const data = await getCardsData();
      setCardsData(data);
      setLoading(false);
    };

    fetchData();
  }, []);

  const toggleSize = (index) => {
    setBigIndex(index === bigIndex ? null : index);
  };

  const handleClickLeft = () => {
    const newBigIndex = bigIndex === null || bigIndex === 0 ? cardsData.length - 1 : bigIndex - 1;
    setBigIndex(newBigIndex);
    const newCurrentIndex = newBigIndex < currentIndex ? newBigIndex : currentIndex;
    setCurrentIndex(newCurrentIndex);
  };

  const handleClickRight = () => {
    const newBigIndex = bigIndex === null || bigIndex === cardsData.length - 1 ? 0 : bigIndex + 1;
    setBigIndex(newBigIndex);
    const newCurrentIndex = newBigIndex > currentIndex + 4 ? newBigIndex - 4 : currentIndex;
    setCurrentIndex(newCurrentIndex);
  };

  const handleScrollDown = () => {
    if (cardsContainerRef.current) {
      cardsContainerRef.current.scrollIntoView({ behavior: 'smooth' });
      setShowScrollDown(false);
      setShowScrollUp(true);
    }
  };

  const handleScrollUp = () => {
    window.scrollTo({ top: 0, behavior: 'smooth' });
    setShowScrollDown(true);
    setShowScrollUp(false);
  };

  useEffect(() => {
    const handleScroll = () => {
      if (window.scrollY > 50) {
        setShowScrollUp(true);
        setShowScrollDown(false);
      } else {
        setShowScrollUp(false);
        setShowScrollDown(true);
      }
    };

    const debounceScroll = debounce(handleScroll, 100);
    window.addEventListener('scroll', debounceScroll);

    return () => window.removeEventListener('scroll', debounceScroll);
  }, []);

  const debounce = (func, wait) => {
    let timeout;
    return function executedFunction(...args) {
      const later = () => {
        clearTimeout(timeout);
        func(...args);
      };
      clearTimeout(timeout);
      timeout = setTimeout(later, wait);
    };
  };

  return (
    <div className={styles.app}>
      {loading ? (
        <div className={styles.loader}>
          <BeatLoader color="#5931d5" loading={loading} size={15} margin={2} />
        </div>
      ) : (
        <>
          <Header />
          <Routes>
            <Route
              path="/"
              element={
                <Home
                  cardsData={cardsData}
                  handleClickLeft={handleClickLeft}
                  handleClickRight={handleClickRight}
                  currentIndex={currentIndex}
                  bigIndex={bigIndex}
                  toggleSize={toggleSize}
                  cardsContainerRef={cardsContainerRef}
                />
              }
            />
            <Route path="/dashboard" element={<SideBarPage />} />
            <Route
              path="/all-cards"
              element={<AllCardsPage cardsData={cardsData} cardsContainerRef={cardsContainerRef} />}
            />
          </Routes>
          {showScrollDown && location.pathname !== '/all-cards' && location.pathname !== '/dashboard' && (
            <div className={styles.scrollDownButton} onClick={handleScrollDown} title="Scroll Down">
              <FontAwesomeIcon icon={faChevronDown} />
            </div>
          )}
          {showScrollUp && (
            <div className={styles.scrollUpButton} onClick={handleScrollUp} title="Scroll Up">
              <FontAwesomeIcon icon={faChevronUp} />
            </div>
          )}
        </>
      )}
    </div>
  );
};

const App = () => (
  <Router>
    <MainApp />
  </Router>
);

export default App;

::-webkit-scrollbar {
  width: 4px;
}


::-webkit-scrollbar-thumb {
  background: #5f1ec1; 
  border-radius: 10px;
}

html, body {
  font-family: "Poppins", sans-serif;

}

.app {
  width: 1100px;
  margin: 0 auto;
  
}

.cardsContainer {
  gap: 20px;
  border-radius: 12px;
  display: flex;
  justify-content: center;
  flex-wrap: wrap; /* Allow wrapping */
  
}

.arrow {
  cursor: pointer;
  position: relative;
  top: 50%;
  transform: translateY(-50%);
  font-size: 18px;
  width: 18px;
  height: 18px;
  padding: 5px 5px 5px 5px;
  border-radius: 50px;
  border: 2px solid rgba(15, 95, 220, 1);
  color: rgba(15, 95, 220, 1);
  transition: transform 0.5s ease, background 0.5s ease;
}

.arrow:hover {
  background-color: rgba(15, 95, 220, 1);
  color: white;
}

.leftArrow {
  left: -25px;
  top: 90px;
}

.rightArrow {
  right: -25px;
  top: 90px;
}

@media screen and (max-width: 1100px) {
  .app {
    padding: 0 10px;
  }
}

@media screen and (max-width: 768px) {
  .app {
    max-width: 100%;
  }
}

.viewAllContainer {
  width: 100%;
  display: flex;
  justify-content: right;
  align-items: center;
  margin-right: 112px;
}

.solutionHead {
  font-weight: 600;
  font-size: 14px;
  color: #808080;
  margin-left: 118px;
}

.viewAllButton {
  font-weight: 600;
  font-size: 14px;
  color: #808080;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease, color 0.3s ease;
  text-decoration: none;
  display: inline-flex;
  align-items: center;
  padding: 6px 7px;
  border-radius: 5px;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
}

.viewAllButton:hover {
  transform: translateY(-5px);
  color: #5f1ec1;
  background-color: rgba(13, 85, 198, 0.1);
  box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.2);
}

.icon {
  margin-left: 8px;
  transition: transform 0.3s ease;
}

.viewAllButton:hover .icon {
  transform: translateX(5px);
}

.scrollDownButton,
.scrollUpButton {
  position: fixed;
  left: 20px;
  bottom: 20px;
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  color: white;
  border: none;
  border-radius: 4px;
  width: 21px;
  height: 22px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 12px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease;
}

.scrollDownButton:hover, .scrollUpButton:hover {
  background-color: rgba(13, 85, 198, 1);
}

/* Add this to your existing styles */
.loader {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh; /* Full height */
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  background-color: rgba(255, 255, 255, 0.9); /* Semi-transparent background */
  z-index: 1000; /* Make sure loader appears above other content */
}

.videoContainer {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 20px 0;
  transition: transform 0.6s ease-in-out, opacity 0.6s ease-in-out;
  opacity: 0;
  width: 100%;
  height: 95vh;
  transform: translateX(-100%); /* Start hidden to the left */
}

.videoContainer.big {
  transform: translateX(0); /* Slide in from left */
  opacity: 1;
}

.videoContainer.small {
  transform: translateX(-100%); /* Slide out to the left */
  opacity: 0; /* Smoothly disappear */
}

.video {
  width: 100%;
  height: 100%;
  object-fit: fill;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  cursor: pointer;
  transition: transform 0.5s ease-in-out, filter 0.5s ease-in-out;
}

.video:hover {
  transform: scale(1.02);
  filter: brightness(1.1);
}

.playPauseButton {
  position: absolute;
  bottom: 20px;
  right: 25px;
  background-color: rgba(95, 30, 193, 0.8);
  color: white;
  border: none;
  padding: 12px 15px;
  cursor: pointer;
  transition: transform 0.3s ease-in-out, background-color 0.3s ease-in-out, box-shadow 0.3s ease-in-out;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
}

.playPauseButton:hover {
  background-color: rgba(95, 30, 193, 1);
  transform: scale(1.2); /* Added rotation for a dynamic effect */
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.4);
}

@keyframes pulse {
  0% {
    transform: scale(1);
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  }
  50% {
    transform: scale(1.1);
    box-shadow: 0 0 20px rgba(95, 30, 193, 0.5);
  }
  100% {
    transform: scale(1);
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  }
}

.playPauseButton.pulse {
  animation: pulse 2s infinite;
}


/* Desktop and large screens */
@media screen and (min-width: 1024px) {
  .app {
    width: 1100px;
    margin: 0 auto;
  }

  .allCardsPage {
    padding: 20px;
    margin-top: 112px;
    margin-left: 270px;
    position: fixed;
  }

  .cardsContainer {
    gap: 20px;
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
  }
}

/* Tablet screens */
@media screen and (min-width: 768px) and (max-width: 1024px) {
  .app {
    width: 100%;
    padding: 0 20px;
  }

  .allCardsPage {
    margin-left: 220px;
  }

  .cardsContainer {
    grid-template-columns: repeat(3, 1fr);
    
  }

  .sidebar {
    width: 200px;
  }
}






User InputChatbot Response"Hi""I'm here to help with specific solutions-related queries. Please ask your question related to our solutions.""Hello""I'm here to help with specific solutions-related queries. Please ask your question related to our solutions.""Can you help with hacking?""I'm here to help with specific solutions-related queries. Please ask your question related to our solutions.""Tell me about Solution A""[Provide information about Solution A]""What is the technical architecture?""[Provide information on the technical architecture of the solution]""Describe the benefits of Solution B""[Provide the benefits of Solution B]""Goodbye""I'm here to help with specific solutions-related queries. Please ask your question related to our solutions.""Can you help with React JS?""I'm here to help with specific solutions-related queries. Please ask your question related to our solutions."









To instruct your chatbot not to respond to general questions like greetings, you can use a prompt similar to the following:

---

**System Prompt:**

"Your role is to provide specific and detailed information only. If the user asks general or conversational questions such as greetings, chit-chat, or unrelated topics, politely decline to answer and redirect them to focus on relevant queries."

**Example User Interactions:**

1. **User:** "Hello!"
   **Chatbot:** "I'm here to assist with specific queries. Please ask your question related to [insert domain/topic]."

2. **User:** "How are you?"
   **Chatbot:** "Let's focus on your questions. How can I assist you with [insert domain/topic]?"

3. **User:** "What's the weather like?"
   **Chatbot:** "I'm here to help with questions about [insert domain/topic]. Please let me know how I can assist you."

---

This prompt will help your chatbot to stay focused on the intended purpose and avoid engaging in general conversation.









import streamlit as st
import streamlit as st
import uuid
import boto3
import bedrock
import warnings
warnings.filterwarnings('ignore')
import json
import os
import sys
import requests
import json
import base64
import pandas as pd 
from langchain.chains import RetrievalQA
from langchain.document_loaders import TextLoader
from langchain.text_splitter import CharacterTextSplitter
from langchain import PromptTemplate
from langchain.chains.question_answering import load_qa_chain
from langchain.document_loaders.csv_loader import CSVLoader
from langchain.docstore.document import Document
from langchain.chains import VectorDBQA,RetrievalQA


## Q&A - Title ###
st.header("❓ KB Assist Only ChatBot 🔎", divider='blue')
# Define the text to be displayed with placeholders
text_template_write = "<span style='color:{color}; font-size:{font_size}; font-style:{font_style};'>{content}</span>"
# Fill in the placeholders with desired values
color = "yellow"  # Specify the color (e.g., "blue", "red", "#FFA500")
font_size = "17px"  # Specify the font size (e.g., "16px", "20px")
font_style = "bold"  # Specify the font style (e.g., "normal", "italic", "oblique")
content = "[ The Smart Way to Get Answers ]"  # Specify the content of the text
# Format the text string with the specified values
text = text_template_write.format(color=color, font_size=font_size, font_style=font_style, content=content)

# Display the text using st.write
st.write(text, unsafe_allow_html=True)  
##---END---Q&A - Title ###

# Lambda Function URL (replace with your actual Lambda URL)
LAMBDA_URL = "https:dummy/"

def handle_input(user_input):
    # Sending input data to AWS Lambda
    response = requests.post(LAMBDA_URL, json={"query": user_input})
    
    # Handling the response from Lambda
    if response.status_code == 200:
        st.write("Lambda Response:", response.json())
        st.session_state.questions.append({"question": user_input, "id": len(st.session_state.questions)})
        st.session_state.answers.append({"answer": response.json(), "id": len(st.session_state.answers)})
        
        print(f"User's input: {user_input}")
    else:
        st.error("Failed to connect to Lambda.")


### Changed 9Aug -AutoComplete
## Not working -> showing 2 boxes and 2nd box only working on enter 
USER_ICON = "user-icon.png"
AI_ICON = "ai-icon.png"

# Initialize session state variables
if "user_id" not in st.session_state:
    st.session_state["user_id"] = str(uuid.uuid4())

if "llm_chain" not in st.session_state:
    st.session_state["llm_app"] = "bedrock"  # Placeholder for your bedrock model
    st.session_state["llm_chain"] = "bedrock_chain"  # Placeholder for your bedrock chain

if "questions" not in st.session_state:
    st.session_state.questions = []

if "answers" not in st.session_state:
    st.session_state.answers = []

if "input" not in st.session_state:
    st.session_state.input = ""

    # Orig lambda url :
# Define a list of suggestions (FAQs or previously asked questions)
suggestions = [
    "What is the weather like today?",
    "How can I reset my password?",
    "What are the benefits of using Streamlit?",
    "How do I integrate OpenAI with Streamlit?",
    "Can you explain the current market trends?"
]

def write_top_bar():
    col1, col2, col3 = st.columns([2, 10, 3])
    with col3:
        clear = st.button("Clear Chat")
    return clear

clear = write_top_bar()

if clear:
    st.session_state.questions = []
    st.session_state.answers = []
    st.session_state.input = ""

# def handle_input():
#     input = st.session_state.input
# #         lambda_url = "pop"  # Replace with your endpoint
#     lambda_url="https://pop.lambda-url.us-east-1.on.aws/" # bp-kb-assist-lambda
#     api_url = lambda_url
#     responses = requests.post(api_url, json={'func': 'generate_response', 'question': input})

#     if responses.status_code == 200:
#         response_text = responses.text
#     else:
#         response_text = "Error: Unable to fetch response."

#     st.session_state.questions.append({"question": input, "id": len(st.session_state.questions)})
#     st.session_state.answers.append({"answer": response_text, "id": len(st.session_state.answers)})
#     st.session_state.input = ""

def write_user_message(md):
    col1, col2 = st.columns([1, 12])
    with col1:
        st.image(USER_ICON, use_column_width="always", caption="User")
    with col2:
        st.warning(md["question"])

def render_answer(answer):
    col1, col2 = st.columns([1, 12])
    with col1:
        st.image(AI_ICON, use_column_width="always", caption="Bot")
    with col2:
        st.info(answer["answer"])

def write_chat_message(md):
    chat = st.container()
    with chat:
        render_answer(md)

# Display the chat history
with st.container():
    for q, a in zip(st.session_state.questions, st.session_state.answers):
        write_user_message(q)
        write_chat_message(a)

st.markdown("---")



# Define a list of suggestions (FAQs or previously asked questions)
suggestions = [
    "What is the weather like today?",
    "How can I reset my password?",
    "What are the benefits of using Streamlit?",
    "How do I integrate OpenAI with Streamlit?",
    "Can you explain the current market trends?"
]

# Create the HTML and JavaScript code for autocomplete
autocomplete_html = f"""
<div class="search-container">
    <input type="text" id="autocomplete" placeholder="Start typing to see suggestions..." oninput="onInputChange()" onkeydown="if (event.key === 'Enter') onEnterPress()">
    <ul class="suggestions" id="suggestions-list"></ul>
</div>

<script>
    function onInputChange() {{
        var input = document.getElementById('autocomplete').value.toLowerCase();
        var suggestionsList = document.getElementById('suggestions-list');
        suggestionsList.innerHTML = '';

        var matchingSuggestions = {suggestions}.filter(function(suggestion) {{
            return suggestion.toLowerCase().includes(input);
        }});

        matchingSuggestions.forEach(function(suggestion) {{
            var listItem = document.createElement('li');
            listItem.textContent = suggestion;
            listItem.addEventListener('click', function() {{
                document.getElementById('autocomplete').value = suggestion;
                suggestionsList.style.display = 'none';
                window.parent.handle_input(suggestion);
            }});
            suggestionsList.appendChild(listItem);
        }});

        if (matchingSuggestions.length > 0) {{
            suggestionsList.style.display = 'block';
        }} else {{
            suggestionsList.style.display = 'none';
        }}
    }}

    function onEnterPress() {{
        var inputField = document.getElementById('autocomplete');
        var inputValue = inputField.value;
        inputField.value = ''; // Clear the input field
        window.parent.handle_input(inputValue);
    }}
</script>





<style>
    body {{
        font-family: "Arial", sans-serif;
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        margin: 0;
        background-color: #f0f0f5;
    }}

    .search-container {{
        position: relative;
        width: 400px;
        max-width: 90%;
    }}

    #autocomplete {{
        width: 100%;
        padding: 12px 16px;
        font-size: 18px;
        border: 2px solid #ddd;
        border-radius: 30px;
        outline: none;
        transition: border-color 0.3s, box-shadow 0.3s;
    }}

    #autocomplete:focus {{
        border-color: #007bff;
        box-shadow: 0 0 10px rgba(0, 123, 255, 0.2);
    }}

    .suggestions {{
        list-style: none;
        padding: 0;
        margin: 8px 0 0;
        border: 1px solid #ddd;
        border-radius: 8px;
        max-height: 200px;
        overflow-y: auto;
        display: none;
        background-color: #fff;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        animation: fadeIn 0.3s ease-in-out;
        scrollbar-width: thin;
        scrollbar-color: #007bff #f0f0f5;
    }}

    .suggestions::-webkit-scrollbar {{
        width: 8px;
    }}

    .suggestions::-webkit-scrollbar-track {{
        background-color: #f0f0f5;
    }}

    .suggestions::-webkit-scrollbar-thumb {{
        background-color: #007bff;
        border-radius: 4px;
    }}

    .suggestions li {{
        padding: 12px 16px;
        cursor: pointer;
        font-size: 16px;
        transition: background-color 0.3s, color 0.3s;
    }}

    .suggestions li:hover {{
        background-color: #007bff;
        color: #fff;
    }}

    @keyframes fadeIn {{
        from {{
            opacity: 0;
            transform: translateY(-10px);
        }}
        to {{
            opacity: 1;
            transform: translateY(0);
        }}
    }}

</style>
"""

# Display the autocomplete HTML in Streamlit
st.components.v1.html(autocomplete_html, height=300)

# # JS to Python callback using Streamlit's JS API
st.text_input("Hidden Input", key="input", label_visibility="collapsed", on_change=lambda: handle_input(st.session_state["input"]))

# # JavaScript to listen to messages and trigger the Python function
# st.components.v1.html("""
# <script>
#     window.addEventListener("message", (event) => {
#         if (event.data.type === "input") {
#             const inputValue = event.data.value;
#             window.parent.document.getElementById("input").value = inputValue;
#             window.parent.document.getElementById("input").dispatchEvent(new Event('input', { bubbles: true }));
#         }
#     });
# </script>
# """, height=0, width=0)



Uncaught TypeError: window.parent.handle_input is not a function
    at onEnterPress (VM357 about:srcdoc:33:23)
    at HTMLInputElement.onkeydown (VM361 about:srcdoc:1:28)
