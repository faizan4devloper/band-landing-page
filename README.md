// Loader.js
import React from 'react';
import './Loader.css';

const Loader = () => {
  return (
    <div className="loader">
      <span className="dot"></span>
      <span className="dot"></span>
      <span className="dot"></span>
    </div>
  );
};

export default Loader;

/* Loader.css */
.loader {
  display: flex;
  align-items: center;
  margin-left: 10px;
}

.dot {
  width: 8px;
  height: 8px;
  margin: 0 2px;
  background-color: #007bff;
  border-radius: 50%;
  animation: blink 1.4s infinite both;
}

.dot:nth-child(2) {
  animation-delay: 0.2s;
}

.dot:nth-child(3) {
  animation-delay: 0.4s;
}

@keyframes blink {
  0%, 80%, 100% {
    opacity: 0;
  }
  40% {
    opacity: 1;
  }
}




import React, { useState } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faPaperPlane, faUser, faRobot } from "@fortawesome/free-solid-svg-icons";
import axios from "axios";
import Loader from "./Loader";
import "./ChatWindow.css";
import UploadDoc from "./UploadDoc";

export function ChatWindow({ onChatResponse }) {
  const [messages, setMessages] = useState([]);
  const [conversationStarted, setConversationStarted] = useState(false);
  const [userInput, setUserInput] = useState("Based on my resume, is there a job in current market? Also help me out on how to apply visa for my family");
  const [loading, setLoading] = useState(false);

  const handleSend = async () => {
    if (userInput.trim()) {
      const newMessage = { sender: "user", text: userInput };
      setMessages([...messages, newMessage]);
      setUserInput("");

      if (!conversationStarted) {
        setConversationStarted(true);
      }

      setLoading(true);

      try {
        const response = await axios.post("https://5toybip0v5.execute-api.us-east-1.amazonaws.com/s1/", {
          type: "both",
          query: newMessage.text
        }, {
          headers: {
            'Content-Type': 'application/json'
          }
        });

        console.log(response);
        const botMessage = { sender: "bot", text: "This is a bot response to: " + response.data.user_query_ans };
        setMessages((prevMessages) => [...prevMessages, botMessage]);

        // Pass the response data to the parent component
        onChatResponse(response.data);

      } catch (error) {
        console.error("Error during API call:", error);
        setMessages((prevMessages) => [
          ...prevMessages,
          { sender: "bot", text: "Error: Unable to send message" },
        ]);
      } finally {
        setLoading(false);
      }
    }
  };

  return (
    <div className="chat-window">
      <div className="chat-content">
        <h3 className="job-live">Live Conversation</h3>
        <div className={`chat-messages ${conversationStarted ? 'show' : ''}`}>
          {messages.map((message, index) => (
            <div key={index} className={`chat-message ${message.sender}`}>
              <FontAwesomeIcon icon={message.sender === 'user' ? faUser : faRobot} />
              {message.text}
            </div>
          ))}
        </div>
        <div className="chat-input">
          <input
            type="text"
            value={userInput}
            onChange={(e) => setUserInput(e.target.value)}
            onKeyPress={(e) => e.key === "Enter" && handleSend()}
            placeholder="Type a message..."
            disabled={loading}
          />
          {loading ? <Loader /> : (
            <button onClick={handleSend}>
              <FontAwesomeIcon icon={faPaperPlane} />
            </button>
          )}
        </div>
        <UploadDoc />
      </div>
    </div>
  );
}

export default ChatWindow;




/* ChatWindow.css */

.chat-window {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f0f0f0;
}

.chat-content {
  width: 100%;
  max-width: 500px;
  background-color: white;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

.job-live {
  background-color: #007bff;
  color: white;
  padding: 10px;
  text-align: center;
  margin: 0;
}

.chat-messages {
  flex: 1;
  padding: 10px;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
}

.chat-message {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
  word-wrap: break-word;
}

.user {
  justify-content: flex-end;
}

.bot {
  justify-content: flex-start;
}

.user-icon {
  margin-right: 10px;
}

.chat-message span a {
  color: #007bff;
  text-decoration: none;
  word-wrap: break-word;
  white-space: pre-wrap;
  overflow-wrap: break-word;
}

.chat-input {
  display: flex;
  align-items: center;
  padding: 10px;
  border-top: 1px solid #ddd;
}

.chat-input input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  margin-right: 10px;
}

.chat-input button {
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  padding: 10px 15px;
  cursor: pointer;
}

.chat-input button:hover {
  background-color: #0056b3;
}

.chat-message.bot {
  display: flex;
  align-items: center;
}
