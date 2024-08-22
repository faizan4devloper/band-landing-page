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
  width: 320px; /* Increased width for better readability */
  height: 500px; /* Increased height */
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

.chatbotMessages {
  flex: 1;
  padding: 10px;
  overflow-y: auto; /* Enable scrolling */
  display: flex;
  flex-direction: column;
}

.userMessage, .botMessage {
  display: flex;
  align-items: flex-start;
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
  max-width: 75%; /* Increased max width */
  background-color: #f1f1f1;
  padding: 10px;
  border-radius: 10px;
  color: #333;
  word-wrap: break-word; /* Ensures long words break properly */
  white-space: pre-wrap; /* Ensures whitespace is respected */
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






import React, { useState, useEffect } from 'react';
import axios from 'axios'; // Import Axios
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faRobot, faTimes, faTrashAlt } from '@fortawesome/free-solid-svg-icons'; // Import trash icon
import { faUser } from '@fortawesome/free-regular-svg-icons';
import { BeatLoader } from 'react-spinners';
import styles from './Chatbot.module.css';

const Chatbot = () => {
  const [isOpen, setIsOpen] = useState(false);
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');
  const [loading, setLoading] = useState(false);
  const [showClearChat, setShowClearChat] = useState(false); // State for clear chat window

  useEffect(() => {
    if (isOpen) {
      setMessages([{ text: 'Hello! How can I assist you today?', sender: 'bot' }]);
    }
  }, [isOpen]);

  const toggleChatbot = () => {
    setIsOpen(!isOpen);
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
      const response = await axios.post('dummy'),  // Replace with your actual endpoint
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

  return (
    <>
      <div className={styles.chatbotIcon} onClick={toggleChatbot}>
        <FontAwesomeIcon icon={faRobot} />
      </div>
      
      {isOpen && (
        <div className={styles.chatbotContainer}>
          <div className={styles.chatbotHeader}>
            <button onClick={() => setShowClearChat(true)} className={styles.clearChatButton}>
              <FontAwesomeIcon icon={faTrashAlt} />
            </button>
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
              onKeyDown={(e) => {
                if (e.key === "Enter") {
                  sendMessage();
                }
              }}
              placeholder="Type your message..."
              disabled={loading}
            />
            <button onClick={sendMessage} disabled={loading} className={styles.sendButton}>
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





.clearChatButton {
  background: none;
  border: none;
  color: white;
  font-size: 16px;
  cursor: pointer;
  margin-right: 10px;
}

.clearChatOverlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
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
  width: 300px; /* Adjust the width as needed */
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.3);
}

.confirmButton {
  background-color: #5f1ec1;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 10px;
}

.cancelButton {
  background-color: #ddd;
  color: #333;
  border: none;
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
}







import React, { useState, useEffect } from 'react';
import axios from 'axios'; // Import Axios
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
      const response = await axios.post(
        'https://7ss0r1iqfc.execute-api.us-east-1.amazonaws.com/v1',  // Replace 'YOUR_ACTUAL_ENDPOINT_URL' with your actual endpoint
        { query: input },  // Adjust the body format as per your Lambda function's expectation
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
              onKeyDown={(e) => {
                if (e.key === "Enter") {
                  sendMessage();
                }
              }}
              placeholder="Type your message..."
              disabled={loading}
            />
            <button onClick={sendMessage} disabled={loading} className={styles.sendButton}>
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
  /*background-color: none;*/
  align-items: center;
  padding: 10px;
  /*font-size: 12px;*/
  /*color: #5f1ec1;*/
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


  
  
  
  
  
  
  
  
