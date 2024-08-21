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
