import React, { useState } from "react";
import axios from "axios";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faPaperPlane,faUser, faRobot } from "@fortawesome/free-solid-svg-icons";
import "./ChatScreen.css";

export function ChatScreen() {
    const [messages, setMessages] = useState([]);
    const [conversationStarted, setConversationStarted] = useState(false);
    const [userInput, setUserInput] = useState("");

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

            "question":newMessage.text
          
                    // sender: "user",
                    // text: userInput,
                }, {
                    headers: {
                        'Content-Type': 'application/json'
                    }
                });

                console.log(response)
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
    return (
        <div className="upload-container">
            <div className="chat-content">
                <h3>Live Conversation</h3>
                <div className={`chat-messages ${conversationStarted ? 'show' : ''}`}>
                    {messages.map((message, index) => (
                        <div key={index} className={`chat-message ${message.sender}`}>
                                        <FontAwesomeIcon className="user-i" icon={message.sender === 'user' ? faUser : faRobot} />

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
        </div>
    );
}

export default ChatScreen;
