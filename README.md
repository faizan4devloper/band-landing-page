// src/components/Job/JobAdvisor.js
import React from "react";
import Uploading from "./Uploading";
import ChatWindow from "./ChatWindow";
import Preview from "./Preview";
import "./JobAdvisor.css";

function JobAdvisor() {
  return (
    <div className="job-advisor">
      <Uploading />
      <ChatWindow />
      <Preview />
    </div>
  );
}

export default JobAdvisor;




// src/components/Job/ChatWindow.js
import React, { useState } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faPaperPlane } from "@fortawesome/free-solid-svg-icons";
import "./ChatWindow.css";

function ChatWindow() {
  const [messages, setMessages] = useState([]);
  const [userInput, setUserInput] = useState("");

  const handleSend = () => {
    if (userInput.trim()) {
      const newMessage = { sender: "user", text: userInput };
      setMessages([...messages, newMessage]);
      setUserInput("");

      // Simulate bot response
      setTimeout(() => {
        const botMessage = { sender: "bot", text: "This is a bot response." };
        setMessages(prevMessages => [...prevMessages, botMessage]);
      }, 1000);
    }
  };

  return (
    <div className="chat-window">
      <h3>Chat Window</h3>
      <div className="chat-messages">
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
  );
}

export default ChatWindow;







// src/components/Job/Uploading.js
import React from "react";

function Uploading() {
  return (
    <div>
      <h3>Uploading</h3>
      <p>Here you can upload your documents.</p>
      {/* Add your upload form or functionality here */}
    </div>
  );
}

export default Uploading;
