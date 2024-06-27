import React from "react";
import Uploading from "./Uploading";
import ChatWindow from "./ChatWindow";
import Preview from "./Preview";
// import "./JobAdvisor.css";

function JobAdvisor() {
  return (
    <div className="job-advisor">
      <ChatWindow />
      <Uploading />
      <Preview />
    </div>
  );
}

export default JobAdvisor;



import React, { useState, useRef } from "react";

import "./UploadDoc.css";

function UploadDoc() {
  const [file, setFile] = useState(null);
    const fileInputRef = useRef(null);


  const handleFileClick = () => {
    fileInputRef.current.click();
  };

  const handleFileChange = (event) => {
    const selectedFile = event.target.files[0];
    if (selectedFile) {
      setFile(selectedFile);
    }
  };

  const handleUpload = () => {
    if (file) {
      // Simulate file upload
      console.log("Uploading file:", file);
      // Reset file input
      setFile(null);
      fileInputRef.current.value = "";
      alert("File uploaded successfully!");
    }
  };
  return (
    <div className="uploading">
      <h4>Upload Documents</h4>
      <button onClick={handleFileClick} className="upload-button">
        Select File
      </button>
      <input
        type="file"
        ref={fileInputRef}
        style={{ display: "none" }}
        onChange={handleFileChange}
      />
      {file && (
        <div className="file-info">
          <p>{file.name}</p>
          
        </div>
      )}
    </div>
  );
}

export default UploadDoc;

import React, { useState } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faPaperPlane } from "@fortawesome/free-solid-svg-icons";
import UploadDoc from "./UploadDoc";
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
      <UploadDoc />
    </div>
  );
}

export default ChatWindow;


import "./Uploading.css";
function Uploading() {
  return (
    <div className="uploading-section">
      <h3>Summary</h3>
      <p>Here you can upload your documents.</p>
      {/* Add your upload form or functionality here */}
    </div>
  );
}

export default Uploading;

import React from 'react';
import "./Preview.css";


function Preview(){
    return <div className="preview">
        <h3>Demo</h3>
    </div>
}

export default Preview;

