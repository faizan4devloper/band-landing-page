import React, { useState } from 'react';
import axios from 'axios';
import './ChatWindow.css'; // Assuming you have some custom styling

const ChatWindow = () => {
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');
  const [loading, setLoading] = useState(false);

  const handleSendMessage = async () => {
    if (input.trim() === "") return;

    // Add user message to the messages array
    setMessages(prevMessages => [
      ...prevMessages,
      { text: input, isBot: false }
    ]);

    setInput("");
    setLoading(true);

    try {
      const response = await axios.post('your-lambda-url', { message: input });
      
      // Debug: Log response data
      console.log(response.data);

      // Check for valid bot answer
      if (response.data && response.data.answer) {
        setMessages(prevMessages => [
          ...prevMessages,
          { text: response.data.answer, isBot: true }
        ]);
      } else {
        // Handle if the response doesn't contain an answer
        setMessages(prevMessages => [
          ...prevMessages,
          { text: "I'm sorry, I didn't understand that.", isBot: true }
        ]);
      }
    } catch (error) {
      console.error("Error sending message:", error);
      setMessages(prevMessages => [
        ...prevMessages,
        { text: "An error occurred. Please try again.", isBot: true }
      ]);
    } finally {
      setLoading(false);
    }
  };

  const handleInputChange = (e) => {
    setInput(e.target.value);
  };

  const handleKeyDown = (e) => {
    if (e.key === 'Enter') {
      handleSendMessage();
    }
  };

  return (
    <div className="chat-window">
      <div className="chat-window-messages">
        {messages.map((msg, index) => (
          <div 
            key={index} 
            className={`chat-message ${msg.isBot ? 'bot-message' : 'user-message'}`}
          >
            {msg.text}
          </div>
        ))}
        {loading && <div className="chat-message bot-message">Bot is typing...</div>}
      </div>
      <div className="chat-window-input">
        <input
          type="text"
          value={input}
          onChange={handleInputChange}
          onKeyDown={handleKeyDown}
          placeholder="Type your message..."
        />
        <button onClick={handleSendMessage}>Send</button>
      </div>
    </div>
  );
};

export default ChatWindow;


import React, { useState } from 'react';
import axios from 'axios';
import './ChatWindow.css'; // Assuming you have some custom styling

const ChatWindow = () => {
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');
  const [loading, setLoading] = useState(false);

  const handleSendMessage = async () => {
    if (input.trim() === "") return;

    // Add user message to the messages array
    setMessages(prevMessages => [
      ...prevMessages,
      { text: input, isBot: false }
    ]);

    setInput("");
    setLoading(true);

    try {
      const response = await axios.post('your-lambda-url', { message: input });
      
      // Debug: Log response data
      console.log(response.data);

      // Check for valid bot answer
      if (response.data && response.data.answer) {
        setMessages(prevMessages => [
          ...prevMessages,
          { text: response.data.answer, isBot: true }
        ]);
      } else {
        // Handle if the response doesn't contain an answer
        setMessages(prevMessages => [
          ...prevMessages,
          { text: "I'm sorry, I didn't understand that.", isBot: true }
        ]);
      }
    } catch (error) {
      console.error("Error sending message:", error);
      setMessages(prevMessages => [
        ...prevMessages,
        { text: "An error occurred. Please try again.", isBot: true }
      ]);
    } finally {
      setLoading(false);
    }
  };

  const handleInputChange = (e) => {
    setInput(e.target.value);
  };

  const handleKeyDown = (e) => {
    if (e.key === 'Enter') {
      handleSendMessage();
    }
  };

  return (
    <div className="chat-window">
      <div className="chat-window-messages">
        {messages.map((msg, index) => (
          <div 
            key={index} 
            className={`chat-message ${msg.isBot ? 'bot-message' : 'user-message'}`}
          >
            {msg.text}
          </div>
        ))}
        {loading && <div className="chat-message bot-message">Bot is typing...</div>}
      </div>
      <div className="chat-window-input">
        <input
          type="text"
          value={input}
          onChange={handleInputChange}
          onKeyDown={handleKeyDown}
          placeholder="Type your message..."
        />
        <button onClick={handleSendMessage}>Send</button>
      </div>
    </div>
  );
};

export default ChatWindow;
