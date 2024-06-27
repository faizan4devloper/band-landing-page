// src/components/Job/Uploading.js
import React, { useState } from "react";
import "./Uploading.css";

function Uploading() {
  const [file, setFile] = useState(null);

  const handleFileChange = (event) => {
    setFile(event.target.files[0]);
  };

  const handleUpload = () => {
    if (file) {
      // Add your file upload logic here
      console.log("File uploaded:", file);
      setFile(null);
    }
  };

  return (
    <div className="uploading">
      <h4>Upload Documents</h4>
      <input type="file" onChange={handleFileChange} />
      <button onClick={handleUpload} disabled={!file}>
        Upload
      </button>
    </div>
  );
}

export default Uploading;



// src/components/Job/ChatWindow.js
import React, { useState } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faPaperPlane } from "@fortawesome/free-solid-svg-icons";
import Uploading from "./Uploading";
import "./ChatWindow.css";

export function ChatWindow() {
  const [messages, setMessages] = useState([]);
  const [conversationStarted, setConversationStarted] = useState(false);
  const [userInput, setUserInput] = useState("");

  const handleSend = () => {
    if (userInput.trim()) {
      const newMessage = { sender: "user", text: userInput };
      setMessages([...messages, newMessage]);
      setUserInput("");
      if (!conversationStarted) {
        setConversationStarted(true);
      }
      // Simulate bot response
      setTimeout(() => {
        const botMessage = { sender: "bot", text: "This is a bot response." };
        setMessages((prevMessages) => [...prevMessages, botMessage]);
      }, 1000);
    }
  };

  return (
    <div className="chat-window">
      <div className="chat-content">
        <h3>Live Conversation</h3>
        <div className={`chat-messages ${conversationStarted ? 'show' : ''}`}>
          {messages.map((message, index) => (
            <div key={index} className={`chat-message ${message.sender}`}>
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
          />
          <button onClick={handleSend}>
            <FontAwesomeIcon icon={faPaperPlane} />
          </button>
        </div>
      </div>
      <Uploading />
    </div>
  );
}

export default ChatWindow;






/* src/components/Job/ChatWindow.css */

.chat-window {
  border: 1px solid #ccc;
  padding: 20px;
  border-radius: 5px;
  display: flex;
  flex-direction: column;
  width: 400px;
  margin: 0 auto;
}

.chat-content {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.chat-messages {
  flex: 1;
  overflow-y: auto;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  padding: 10px;
  border-radius: 5px;
  max-height: 300px;
  transition: max-height 0.5s ease-in-out;
}

.chat-messages.show {
  max-height: 500px;
}

.chat-input {
  display: flex;
  margin-top: 10px;
}

.chat-input input {
  flex: 1;
  padding: 10px;
  margin-right: 10px;
}

.chat-message.user {
  text-align: right;
  background-color: #e0f7fa;
  margin: 5px;
  padding: 10px;
  border-radius: 5px;
  animation: fadeIn 0.5s;
}

.chat-message.bot {
  text-align: left;
  background-color: #ffebee;
  margin: 5px;
  padding: 10px;
  border-radius: 5px;
  animation: fadeIn 0.5s;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

/* src/components/Job/Uploading.css */

.uploading {
  margin-top: 20px;
  border: 1px solid #ccc;
  padding: 10px;
  border-radius: 5px;
  text-align: center;
  transition: transform 0.3s ease-in-out;
}

.uploading:hover {
  transform: scale(1.05);
}

.uploading h4 {
  margin-bottom: 10px;
}

.uploading input {
  margin-bottom: 10px;
}

.uploading button {
  padding: 10px 20px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.uploading button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

