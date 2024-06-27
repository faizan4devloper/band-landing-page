import React, { useState } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faPaperPlane } from "@fortawesome/free-solid-svg-icons";

import "./ChatWindow.css";

export function ChatWindow(){
    const [messages, setMessages] = useState([]);
    const [conversationStarted, setConversationStarted] = useState(false)
    const [userInput, setUserInput] = useState("");
    const handleSend = () => {
        if (userInput.trim()) {
            const newMessage = { sender: "user", text: userInput };
            setMessages([...messages, newMessage]);
            setUserInput("");
            if(!conversationStarted){
                setConversationStarted(true)
            }
            // Simulate bot response
            setTimeout(() => {
                const botMessage = { sender: "bot", text: "This is a bot response." };
                setMessages(prevMessages => [...prevMessages, botMessage]);
            }, 1000);
        }
    };
    
    return <div className="chat-window">
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
                    <button onClick={handleSend}><FontAwesomeIcon icon={faPaperPlane} /></button>
                </div>
            </div>
    </div>
}

export default ChatWindow;
