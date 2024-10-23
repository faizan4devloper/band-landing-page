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


.chat-window {
  width: 400px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  border: 1px solid #ddd;
  border-radius: 10px;
  overflow: hidden;
}

.chat-window-messages {
  flex: 1;
  padding: 10px;
  background-color: #f5f5f5;
  overflow-y: auto;
}

.chat-message {
  margin: 10px 0;
  padding: 8px 12px;
  border-radius: 20px;
  max-width: 70%;
  word-wrap: break-word;
}

.user-message {
  background-color: #0084ff;
  color: white;
  align-self: flex-end;
}

.bot-message {
  background-color: #e0e0e0;
  color: black;
  align-self: flex-start;
}

.chat-window-input {
  display: flex;
  padding: 10px;
  background-color: #fff;
  border-top: 1px solid #ddd;
}

.chat-window-input input {
  flex: 1;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 5px;
  margin-right: 10px;
}

.chat-window-input button {
  padding: 8px 15px;
  background-color: #0084ff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.chat-window-input button:hover {
  background-color: #006bb3;
}
