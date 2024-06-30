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
                const response = await axios.post("https://2amqzrdnuj.execute-api.us-east-1.amazonaws.com/dev", {
                    question: newMessage.text
                }, {
                    headers: {
                        'Content-Type': 'application/json'
                    }
                });

                const botMessage = { sender: "bot", text: response.data.text };
                const botMessage1 = { sender: "bot", text: response.data.file_url };
                setMessages((prevMessages) => [...prevMessages, botMessage, botMessage1]);

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

    const renderMessageText = (text) => {
        const urlRegex = /(https?:\/\/[^\s]+)/g;
        const parts = text.split(urlRegex).map((part, index) =>
            urlRegex.test(part) ? <a key={index} href={part} target="_blank" rel="noopener noreferrer">{part}</a> : part
        );
        return <span>{parts}</span>;
    };

    return (
        <div className="upload-container">
            <div className="chat-content">
                <h3>Live Conversation</h3>
                <div className={`chat-messages ${conversationStarted ? 'show' : ''}`}>
                    {messages.map((message, index) => (
                        <div key={index} className={`chat-message ${message.sender}`}>
                            <FontAwesomeIcon className="user-icon" icon={message.sender === 'user' ? faUser : faRobot} />
                            {renderMessageText(message.text)}
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









/* ChatScreen.css */

.upload-container {
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

h3 {
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
