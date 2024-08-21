import React, { useState, useEffect } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faRobot, faTimes } from '@fortawesome/free-solid-svg-icons';
import { faUser } from '@fortawesome/free-regular-svg-icons';
import { BeatLoader } from 'react-spinners';
import styles from './Chatbot.module.css';

const Chatbot = () => {
  const [isOpen, setIsOpen] = useState(false);
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    if (isOpen) {
      setMessages([{ text: 'Hello! How can I assist you today?', sender: 'bot' }]);
    }
  }, [isOpen]);

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

    // Add a loading indicator as a bot message before the actual response
    setMessages((prevMessages) => [
      ...prevMessages,
      { text: '', sender: 'bot', loading: true },
    ]);

    // Simulate bot response after a delay
    setTimeout(() => {
      const botMessage = { text: 'This is a bot response.', sender: 'bot' };
      setMessages((prevMessages) => {
        // Remove the loading indicator and add the actual bot message
        return prevMessages.map((msg) =>
          msg.loading ? botMessage : msg
        );
      });
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
                  icon={message.sender === 'user' ? faUser : faRobot}
                  className={styles.icon}
                />
                <div className={styles.messageText}>
                  {message.loading ? (
                    <BeatLoader color="#5f1ec1" size={8} />
                  ) : (
                    message.text
                  )}
                </div>
              </div>
            ))}
          </div>

          <div className={styles.chatbotInput}>
            <input
              type="text"
              value={input}
              onChange={(e) => setInput(e.target.value)}
              placeholder="Type your message..."
              disabled={loading}
            />
            <button onClick={sendMessage} disabled={loading} className={styles.sendButton}>
              {loading ? (
                <BeatLoader color="#fff" size={8} />
              ) : (
                <FontAwesomeIcon icon={faPaperPlane} />
              )}
            </button>
          </div>
        </div>
      )}
    </>
  );
};

export default Chatbot;



.loader {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px;
}

.messageText {
  max-width: 70%;
  background-color: #f1f1f1;
  padding: 10px;
  border-radius: 10px;
  color: #333;
  display: flex;
  align-items: center; /* To center the loader vertically */
}











import React, { useState, useEffect } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faRobot, faTimes } from '@fortawesome/free-solid-svg-icons';
import { faUser } from '@fortawesome/free-regular-svg-icons';
import { BeatLoader } from 'react-spinners'; // Import BeatLoader for the loader
import styles from './Chatbot.module.css';

const Chatbot = () => {
  const [isOpen, setIsOpen] = useState(false);
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    if (isOpen) {
      setMessages([{ text: 'Hello! How can I assist you today?', sender: 'bot' }]);
    }
  }, [isOpen]);

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
                  icon={message.sender === 'user' ? faUser : faRobot}
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
              disabled={loading} // Disable input when loading
            />
            <button onClick={sendMessage} disabled={loading} className={styles.sendButton}>
              {loading ? (
                <BeatLoader color="#fff" size={8} />
              ) : (
                <FontAwesomeIcon icon={faPaperPlane} />
              )}
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
  justify-content:right;
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
  margin: 0 8px;
  font-size: 15px;
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
  display: flex;
  align-items: center;
  justify-content: center;
}

.chatbotInput button:hover {
  background-color: #4a0fa5;
}

.loader {
  display: flex;
  justify-content: center;
  background-color: none;
  align-items: center;
  padding: 10px;
  font-size: 12px;
  color: #5f1ec1;
}




