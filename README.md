import React, { useState, useEffect, useRef } from 'react';
import axios from 'axios';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faWandSparkles, faUser, faComments, faTimes } from '@fortawesome/free-solid-svg-icons';
import { BeatLoader } from 'react-spinners';
import styles from './Chatbot.module.css';

const Chatbot = () => {
  const [input, setInput] = useState('');
  const [messages, setMessages] = useState([]);
  const [loading, setLoading] = useState(false);
  const [isChatVisible, setChatVisible] = useState(false); // State for visibility

  const messagesEndRef = useRef(null);

  useEffect(() => {
    // Auto-scroll to the latest message
    if (messagesEndRef.current) {
      messagesEndRef.current.scrollIntoView({ behavior: 'smooth' });
    }
  }, [messages]);

  const handleSubmit = async (e) => {
    e.preventDefault();
    if (input.trim() === '') return;

    const userMessage = { type: 'user', text: input };
    setMessages((prevMessages) => [...prevMessages, userMessage]);
    setInput('');
    setLoading(true);

    try {
      const response = await axios.post('dummy', {
        question: input,
      }, {
        headers: {
          'Content-Type': 'application/json',
        },
      });

      const parsedBody = JSON.parse(response.data.body);
      const answer = parsedBody.answer || 'No answer available for this question.';
      const source = parsedBody.source || 'No source available';

      const botMessage = {
        type: 'bot',
        text: `${answer} (Source: ${source})`,
      };
      setMessages((prevMessages) => [...prevMessages, botMessage]);

    } catch (error) {
      console.error('Error fetching data:', error);
      const errorMessage = { type: 'bot', text: 'Something went wrong. Please try again later.' };
      setMessages((prevMessages) => [...prevMessages, errorMessage]);
    } finally {
      setLoading(false);
    }
  };

  const toggleChatVisibility = () => {
    setChatVisible(!isChatVisible); // Toggle visibility
  };

  return (
    <div className={styles.chatContainer}>
      {/* Conversation Icon */}
      <div className={styles.iconContainer} onClick={toggleChatVisibility}>
        <FontAwesomeIcon icon={faComments} className={styles.conversationIcon} />
      </div>

      {/* Chat Window - Modal Style */}
      {isChatVisible && (
        <div className={styles.chatWindow}>
          <div className={styles.chatHeader}>
            <p className={styles.chatHeading}>Chatbot</p>
            <button className={styles.closeButton} onClick={toggleChatVisibility}><FontAwesomeIcon icon={faTimes}/></button>
          </div>
          <div className={styles.messages}>
            {messages.map((message, index) => (
              <div
                key={index}
                className={message.type === 'user' ? styles.userMessage : styles.botMessage}
              >
                <FontAwesomeIcon
                  icon={message.type === 'user' ? faUser : faWandSparkles}
                  className={styles.icon}
                />
                <div className={styles.messageText}>
                  {message.text}
                </div>
              </div>
            ))}
            {loading && (
              <div className={styles.botMessage}>
                <FontAwesomeIcon icon={faWandSparkles} className={styles.icon} />
                <div className={styles.messageText}>
                  <BeatLoader color="#5f1ec1" size={8} />
                </div>
              </div>
            )}
            <div ref={messagesEndRef} />
          </div>

          {/* Input Form */}
          <form onSubmit={handleSubmit} className={styles.inputForm}>
            <input
              type="text"
              value={input}
              onChange={(e) => setInput(e.target.value)}
              placeholder="Ask a question..."
              className={styles.inputField}
              disabled={loading}
            />
            <button type="submit" className={styles.submitButton} title="Send">
              <FontAwesomeIcon icon={faPaperPlane} />
            </button>
          </form>
        </div>
      )}
    </div>
  );
};

export default Chatbot;



:root {
  --primary-color: #5f1ec1;
  --secondary-color: rgba(15, 95, 220, 1);
  --background-color: #ffffff;
  --inp-text-color: #00000;
  --chat-text-color: #fff;
  --scrollbar-color: rgba(15, 95, 220, 1);
  --scrollbar-background: #dcdcdc;
  --button-background-color: rgba(13, 85, 198, 0.1);
  --button-hover-color: #5f1ec1;
  --placeholder-color: #a9a9a9; /* Light theme placeholder color */
  --icon-color: #000000;
}

/* Dark Theme */
[data-theme="dark"] {
  --primary-color: #9d66f5;
  --secondary-color: #c1a1f2;
  --background-color: #1a1a2e;
  --inp-text-color: #ffffff;
  --text-color: #ffffff;
  --scrollbar-color: #5f1ec1;
  --scrollbar-background: #333333;
  --button-background-color: rgba(95, 30, 193, 0.8);
  --button-hover-color: #c1a1f2;
  --placeholder-color: #555555; /* Dark theme placeholder color */
}

.chatContainer {
    position: fixed;
    bottom: 20px;
    color:#fff;
    /*padding: 14px;*/
    /*border-radius: 30%;*/
    /*background: linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%);*/
    right: 20px;
    z-index: 1000;
}

.iconContainer{
  cursor: pointer;  
  padding: 14px;
    border-radius: 30%;
    background: linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%);
}

/* Chat Window - Modal Style */
.chatWindow {
  position: fixed;
  bottom: 80px;
  right: 20px;
  width: 320px;
  height: 420px;
  background-color: var(--background-color);
  border-radius: 8px;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.3);
  display: flex;
  flex-direction: column;
  transition: all 0.3s ease;
}

.chatHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: linear-gradient(90deg, var(--primary-color) 0%, var(--secondary-color) 100%);
  color: var(--chat-text-color);
  padding: 5px;
  border-top-left-radius: 8px;
  border-top-right-radius: 8px;
}

.chatHeading{
  font-size: 14px;
  font-weight: bold;
  text-align:center;
}


.closeButton {
  background: none;
  border: none;
  color: var(--chat-text-color);
  font-size: 16px;
  cursor: pointer;
}

.messages {
  flex: 1;
  padding: 10px;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
}

.userMessage,
.botMessage {
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
  color: #fff;
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

.userMessage .messageText {
  background-color: #d1e7ff;
}

.inputForm {
  display: flex;
  border-top: 1px solid #ddd;
  padding: 10px;
}

.inputField {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  color: #000;
  outline: none;
}

.submitButton {
  background: linear-gradient(90deg, var(--primary-color) 0%, var(--secondary-color) 100%);
  color: var(--chat-text-color);
  border: none;
  padding: 10px;
  border-radius: 4px;
  margin-left: 10px;
  cursor: pointer;
}


.messages::-webkit-scrollbar{
  width:6px;
}
.messages::-webkit-scrollbar-thumb{
background-color: rgba(15, 95, 220, 1);
border-radius: 10px
}
