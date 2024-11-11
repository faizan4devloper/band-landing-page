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
    // Dynamically set the question_id (e.g., Q1, Q2, etc.)
    const questionId = `Q${Math.floor(Math.random() * 9) + 1}`; // Randomly generates Q1-Q9

    const payload = {
      question: input,
      question_id: questionId,  // Dynamically assigned question_id
    };

    const response = await axios.post('dummy', payload, {
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
