import React, { useState, useEffect } from 'react';
import axios from 'axios'; // Import Axios
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faRobot, faTimes } from '@fortawesome/free-solid-svg-icons';
import { faUser } from '@fortawesome/free-regular-svg-icons';
import { BeatLoader } from 'react-spinners';
import styles from './Chatbot.module.css';

const Chatbot = () => {
  const [isOpen, setIsOpen] = useState(false);
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    if (isOpen) {
      setMessages([{ text: 'Hello! How can I assist you today?', sender: 'bot' }]);
    }
  }, [isOpen]);

  const toggleChatbot = () => {
    setIsOpen(!isOpen);
  };

  const sendMessage = async () => {
    if (input.trim() === '') return;

    // Add user message
    const userMessage = { text: input, sender: 'user' };
    setMessages([...messages, userMessage]);
    setInput('');
    setLoading(true);

    // Add a loading indicator as a bot message before the actual response
    setMessages((prevMessages) => [
      ...prevMessages,
      { text: '', sender: 'bot', loading: true },
    ]);

    try {
      // Send user message to your API
      const response = await axios.post('YOUR_API_ENDPOINT', { message: input });

      // Simulate bot response after a delay
      const botMessage = { text: response.data.reply, sender: 'bot' };

      setMessages((prevMessages) =>
        prevMessages.map((msg) =>
          msg.loading ? botMessage : msg
        )
      );
    } catch (error) {
      // Handle error (you might want to display an error message to the user)
      console.error('Error sending message:', error);
    } finally {
      setLoading(false);
    }
  };

  return (
    <>
      <div className={styles.chatbotIcon} onClick={toggleChatbot}>
        <FontAwesomeIcon icon={faRobot} />
      </div>
      
      {isOpen && (
        <div className={styles.chatbotContainer}>
          <div className={styles.chatbotHeader}>
            <button onClick={toggleChatbot} className={styles.closeButton}>
              <FontAwesomeIcon icon={faTimes} />
            </button>
          </div>

          <div className={styles.chatbotMessages}>
            {messages.map((message, index) => (
              <div
                key={index}
                className={
                  message.sender === 'user' ? styles.userMessage : styles.botMessage
                }
              >
                <FontAwesomeIcon
                  icon={message.sender === 'user' ? faUser : faRobot}
                  className={styles.icon}
                />
                <div className={styles.messageText}>
                  {message.loading ? (
                    <BeatLoader color="#5f1ec1" size={8} />
                  ) : (
                    message.text
                  )}
                </div>
              </div>
            ))}
          </div>

          <div className={styles.chatbotInput}>
            <input
              type="text"
              value={input}
              onChange={(e) => setInput(e.target.value)}
              onKeyDown={(e) => {
                if (e.key === "Enter") {
                  sendMessage();
                }
              }}
              placeholder="Type your message..."
              disabled={loading}
            />
            <button onClick={sendMessage} disabled={loading} className={styles.sendButton}>
              <FontAwesomeIcon icon={faPaperPlane} />
            </button>
          </div>
        </div>
      )}
    </>
  );
};

export default Chatbot;







import React, { useState } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faPaperPlane, faUser, faRobot, faTimes } from "@fortawesome/free-solid-svg-icons";
import axios from "axios";
import "./Chatbot.css"; // Assume you have a CSS file for styling
import { Loader } from "./Loader"; // Assume you have a Loader component
import UploadDoc from "./UploadDoc"; // Assume you have a document upload component

export function Chatbot() {
  const [messages, setMessages] = useState([]);
  const [conversationStarted, setConversationStarted] = useState(false);
  const [input, setInput] = useState("Based on my resume, is there a job in the current market? Also, help me out on how to apply for a visa for my family.");
  const [loading, setLoading] = useState(false);

  const handleSend = async () => {
    if (input.trim()) {
      const newMessage = { sender: "user", text: input };
      setMessages([...messages, newMessage]);
      setInput("");

      if (!conversationStarted) {
        setConversationStarted(true);
      }

      setLoading(true);

      try {
        const response = await axios.post("https://5toybip0v5.execute-api.us-east-1.amazonaws.com/s1/", {
          func: "generate_response",
          question: newMessage.text
        }, {
          headers: {
            'Content-Type': 'application/json'
          }
        });

        console.log(response);
        const botMessage = { sender: "bot", text: response.data.body };
        setMessages((prevMessages) => [...prevMessages, botMessage]);

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
    <div className="chatbot">
      <div className="chatbot-header">
        <h3>Live Conversation</h3>
        <button className="close-button">
          <FontAwesomeIcon icon={faTimes} />
        </button>
      </div>
      <div className={`chatbot-messages ${conversationStarted ? 'show' : ''}`}>
        {messages.map((message, index) => (
          <div key={index} className={`chatbot-message ${message.sender}`}>
            <FontAwesomeIcon icon={message.sender === 'user' ? faUser : faRobot} />
            {message.text}
          </div>
        ))}
      </div>
      <div className="chatbot-input">
        <input
          type="text"
          value={input}
          onChange={(e) => setInput(e.target.value)}
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
  );
}

export default Chatbot;




import React, { useState } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faPaperPlane, faUser, faRobot } from "@fortawesome/free-solid-svg-icons";
import axios from "axios";
import "./ChatWindow.css";
import { Loader } from "./Loader";

import UploadDoc from "./UploadDoc"

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
        const botMessage = { sender: "bot", text:response.data.user_query_ans };
        setMessages((prevMessages) => [...prevMessages, botMessage]);

        // Pass the response data to the parent component
        onChatResponse(response.data);

      } catch (error) {
        console.error("Error during API call:", error);
        setMessages((prevMessages) => [
          ...prevMessages,
          { sender: "bot", text: "Error: Unable to send message" },
        ]);
      }
      finally {
        setLoading(false);
      }
    }
  };

  return (
    <div className="chat-window">
      <div className="chat-content">
        <h3>Live Conversation</h3>
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
        <UploadDoc/>
      </div>
    </div>
  );
}

export default ChatWindow;
