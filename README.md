import React, { useState } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';

const MainContent = () => {
  const [input, setInput] = useState('');
  const [messages, setMessages] = useState([]);

  // Handle form submission and fetch answer from the API
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

      // Parse the response and extract the answer
      const parsedBody = JSON.parse(response.data.body);

      // Add chatbot's response to the conversation
      const botMessage = {
        type: 'bot',
        text: parsedBody.answer?.textual || 'No answer available for this question.'
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
    <div className={styles.chatbotContainer}>
      <div className={styles.chatWindow}>
        {messages.length > 0 ? (
          messages.map((message, index) => (
            <div key={index} className={message.type === 'user' ? styles.userMessage : styles.botMessage}>
              {message.text}
            </div>
          ))
        ) : (
          <div className={styles.emptyChat}>Ask a question to get started!</div>
        )}
      </div>
      <form onSubmit={handleSubmit} className={styles.inputForm}>
        <input
          type="text"
          className={styles.inputField}
          placeholder="Type your question..."
          value={input}
          onChange={(e) => setInput(e.target.value)}
        />
        <button type="submit" className={styles.sendButton}>Send</button>
      </form>
    </div>
  );
};

export default MainContent;
