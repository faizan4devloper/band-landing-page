import React, { useState } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';

const MainContent = () => {
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

export default MainContent;
