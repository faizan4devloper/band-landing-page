import React, { useState } from 'react';
import axios from 'axios';
import styles from './Chatbot.module.css';

const Chatbot = () => {
  const [input, setInput] = useState('');
  const [messages, setMessages] = useState([]);

  // Handle form submission to send a question
  const handleSubmit = async (e) => {
    e.preventDefault();
    if (input.trim() === '') return;

    // Add user's question to the conversation
    const userMessage = { type: 'user', text: input };
    setMessages([...messages, userMessage]);

    try {
      // Send POST request to the API
      const response = await axios.post('dummy', {
        question: input
      }, {
        headers: {
          'Content-Type': 'application/json'
        },
      });

      console.log('Full API Response:', response);

      // Parse the stringified body to extract answer and source
      const parsedBody = JSON.parse(response.data.body);

      // Extract answer and source
      const answer = parsedBody.answer || 'No answer available for this question.';
      const source = parsedBody.source || 'No source available';

      // Add chatbot's response to the conversation
      const botMessage = {
        type: 'bot',
        text: `${answer} (Source: ${source})`
      };
      setMessages((prevMessages) => [...prevMessages, botMessage]);

    } catch (error) {
      console.error('Error fetching data:', error);
      const errorMessage = { type: 'bot', text: 'Something went wrong. Please try again later.' };
      setMessages((prevMessages) => [...prevMessages, errorMessage]);
    }

    // Clear input field
    setInput('');
  };

  return (
    <div className={styles.chatContainer}>
      <div className={styles.chatWindow}>
        {messages.map((message, index) => (
          <div
            key={index}
            className={message.type === 'user' ? styles.userMessage : styles.botMessage}
          >
            {message.text}
          </div>
        ))}
      </div>
      <form onSubmit={handleSubmit} className={styles.inputForm}>
        <input
          type="text"
          value={input}
          onChange={(e) => setInput(e.target.value)}
          placeholder="Ask a question..."
          className={styles.inputField}
        />
        <button type="submit" className={styles.submitButton}>Send</button>
      </form>
    </div>
  );
};

export default Chatbot;




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
      const response = await axios.post('dummy', // Replace with your actual endpoint
        { 
          question: input 
          
        },
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
                <FontAwesomeIcon icon={faChevronDown} onClick={minimizeChatbot}/>
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
