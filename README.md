import React, { useState, useEffect, useRef } from 'react';
import axios from 'axios';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faComments, faTimes } from '@fortawesome/free-solid-svg-icons';
import { BeatLoader } from 'react-spinners';
import styles from './Chatbot.module.css';

const Chatbot = ({ initialQuestion }) => {
  const [input, setInput] = useState('');
  const [messages, setMessages] = useState([]);
  const [loading, setLoading] = useState(false);
  const [isChatVisible, setChatVisible] = useState(false);
  const messagesEndRef = useRef(null);

  // Scroll to the bottom when new messages are added
  useEffect(() => {
    if (messagesEndRef.current) {
      messagesEndRef.current.scrollIntoView({ behavior: 'smooth' });
    }
  }, [messages]);

  // Check if there's a new question from MainContent
  useEffect(() => {
    if (initialQuestion) {
      const userMessage = { type: 'user', text: initialQuestion.question };
      const botMessage = { type: 'bot', text: initialQuestion.answer.textualResponse.join(' ') };
      setMessages((prevMessages) => [...prevMessages, userMessage, botMessage]);
    }
  }, [initialQuestion]);

  const handleSubmit = async (e) => {
    e.preventDefault();
    if (input.trim() === '') return;

    const userMessage = { type: 'user', text: input };
    setMessages((prevMessages) => [...prevMessages, userMessage]);
    setInput('');
    setLoading(true);

    try {
      const response = await axios.post('dummy', { question: input }, {
        headers: { 'Content-Type': 'application/json' },
      });

      const parsedBody = JSON.parse(response.data.body);
      const answer = parsedBody.answer || 'No answer available for this question.';
      const botMessage = {
        type: 'bot',
        text: answer,
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
    setChatVisible(!isChatVisible);
  };

  return (
    <div className={styles.chatContainer}>
      <div className={styles.iconContainer} onClick={toggleChatVisibility}>
        <FontAwesomeIcon icon={faComments} className={styles.conversationIcon} />
      </div>

      {isChatVisible && (
        <div className={styles.chatWindow}>
          <div className={styles.chatHeader}>
            <p className={styles.chatHeading}>Chatbot</p>
            <button className={styles.closeButton} onClick={toggleChatVisibility}>
              <FontAwesomeIcon icon={faTimes} />
            </button>
          </div>
          <div className={styles.messagesContainer}>
            {messages.map((msg, idx) => (
              <div key={idx} className={msg.type === 'user' ? styles.userMessage : styles.botMessage}>
                {msg.text}
              </div>
            ))}
            {loading && (
              <div className={styles.loaderWrapper}>
                <BeatLoader color="rgb(15, 95, 220)" loading={loading} size={8} />
              </div>
            )}
            <div ref={messagesEndRef} />
          </div>
          <form onSubmit={handleSubmit} className={styles.inputContainer}>
            <input
              type="text"
              placeholder="Type your message..."
              value={input}
              onChange={(e) => setInput(e.target.value)}
              className={styles.inputField}
            />
            <button type="submit" className={styles.sendButton}>
              <FontAwesomeIcon icon={faPaperPlane} />
            </button>
          </form>
        </div>
      )}
    </div>
  );
};

export default Chatbot;
