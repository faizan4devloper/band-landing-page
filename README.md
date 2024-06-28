import React, { useState, useEffect, useRef } from "react";
import axios from "axios";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faPaperPlane, faUser, faRobot } from "@fortawesome/free-solid-svg-icons";
import "./ChatScreen.css";

export function ChatScreen() {
    const [messages, setMessages] = useState([]);
    const [conversationStarted, setConversationStarted] = useState(false);
    const [userInput, setUserInput] = useState("");
    const messagesEndRef = useRef(null);

    const handleSend = async () => {
        if (userInput.trim()) {
            const newMessage = { sender: "user", text: userInput };
            setMessages([...messages, newMessage]);
            setUserInput("");

            if (!conversationStarted) {
                setConversationStarted(true);
            }

            try {
                const response = await axios.post("demo", {
                    question: newMessage.text
                }, {
                    headers: {
                        'Content-Type': 'application/json'
                    }
                });

                const botMessage = { sender: "bot", text: "This is a bot response to: " + response.data.text };
                setMessages((prevMessages) => [...prevMessages, botMessage]);

            } catch (error) {
                console.error("Error during API call:", error);
                setMessages((prevMessages) => [
                    ...prevMessages,
                    { sender: "bot", text: "Error: Unable to send message" },
                ]);
            }
        }
    };

    useEffect(() => {
        if (messages.length > 0) {
            messagesEndRef.current?.scrollIntoView({ behavior: 'smooth' });
        }
    }, [messages]);

    return (
        <div className="upload-container">
            <div className="chat-content">
                <h3>Live Conversation</h3>
                <div className={`chat-messages ${conversationStarted ? 'show' : ''}`}>
                    {messages.map((message, index) => (
                        <div key={index} className={`chat-message ${message.sender}`}>
                            <FontAwesomeIcon className="user-icon" icon={message.sender === 'user' ? faUser : faRobot} />
                            {message.text}
                        </div>
                    ))}
                    <div ref={messagesEndRef} />
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
        </div>
    );
}

export default ChatScreen;








.upload-container {
  background-color: rgba(0, 0, 0, 0.5);
  margin: 80px 0 0 10px;
  border-radius: 10px;
  width: 450px;
}

.upload-container h3 {
  text-align: center;
  font-size: 1.5rem;
  line-height: 1.2;
  font-weight: 600;
  background: -webkit-gradient(linear, left top, right top, color-stop(-19.51%, #7abef7), color-stop(36.51%, #4080f5), to(#572ac2));
  background: -webkit-linear-gradient(left, #7abef7 -19.51%, #4080f5 36.51%, #572ac2 100%);
  background: -o-linear-gradient(left, #7abef7 -19.51%, #4080f5 36.51%, #572ac2 100%);
  background: linear-gradient(90deg, #7abef7 -19.51%, #4080f5 36.51%, #572ac2 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  text-fill-color: transparent;
  text-transform: inherit !important;
}

h3 {
  text-align: center;
  margin: 12px 0;
  color: #fff;
}

.chat-content {
  display: flex;
  flex-direction: column;
  margin-top: 30px;
}

.chat-messages {
  flex: 1;
  max-height: 300px;
  opacity: 0;
  padding: 10px;
  margin: 20px;
  border-radius: 5px;
  overflow-y: auto;
  background-color: #f9f9f9;
  transition: opacity 0.5s ease;
}

.chat-messages.show {
  opacity: 1;
}

.chat-message {
  display: flex;
  align-items: center;
  padding: 10px;
  margin: 5px 0;
  border-radius: 4px;
  opacity: 0;
  animation: fadeIn 0.5s forwards;
}

@keyframes fadeIn {
  to {
    opacity: 1;
  }
}

.chat-message.user {
  background: linear-gradient(90deg, #1f77f6 0%, #6f36cd 100%);
  font-size: 12px;
  color: #fff;
  text-align: right;
  justify-content: flex-end;
  padding-right: 15px;
}

.chat-message.bot {
  background-color: purple;
  font-size: 12px;
  color: #fff;
  text-align: left;
  justify-content: flex-start;
}

.chat-message .user-icon {
  margin-right: 8px;
}

.chat-input {
  display: flex;
  padding: 10px;
  margin: 20px;
  border-radius: 5px;
  border-top: 1px solid #ccc;
  background-color: #fff;
}

.chat-input input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.chat-input button {
  margin-left: 10px;
  padding: 10px 10px;
  border: none;
  border-radius: 24px;
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  color: #fff;
  cursor: pointer;
  transition: background-color 0.3s ease-in-out;
}

.chat-input button:hover {
  background-color: #0056b3;
}
