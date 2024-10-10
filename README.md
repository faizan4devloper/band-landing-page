npm install react-chat-widget

import React, { useState } from 'react';
import { Widget, addResponseMessage } from 'react-chat-widget';
import 'react-chat-widget/lib/styles.css';
import { FiMessageSquare } from 'react-icons/fi';
import { IoCloseSharp } from 'react-icons/io5';
import styles from './ChatBot.module.css'; // Assuming you're using CSS modules

function ChatBot() {
    const [isOpen, setIsOpen] = useState(false);

    const handleToggle = () => {
        setIsOpen(!isOpen);
    };

    const handleNewUserMessage = (message) => {
        console.log(`New message incoming! ${message}`);
        // Handle the user message, you can send it to a backend here.
        addResponseMessage("I'm an advanced bot. How can I assist you?");
    };

    return (
        <>
            {/* Chatbot Icon */}
            <div className={styles.chatIcon} onClick={handleToggle}>
                {isOpen ? <IoCloseSharp size={30} /> : <FiMessageSquare size={30} />}
            </div>

            {/* Chatbot Window */}
            {isOpen && (
                <div className={styles.chatWindow}>
                    <Widget
                        title="AI Ninja"
                        subtitle="Ask me anything!"
                        handleNewUserMessage={handleNewUserMessage}
                    />
                </div>
            )}
        </>
    );
}

export default ChatBot;


/* ChatBot.module.css */
.chatIcon {
    position: fixed;
    bottom: 20px;
    right: 20px;
    background-color: #7ca2e1;
    color: white;
    padding: 15px;
    border-radius: 50%;
    cursor: pointer;
    transition: all 0.3s ease;
}

.chatIcon:hover {
    background-color: #5c88d1;
}

.chatWindow {
    position: fixed;
    bottom: 80px;
    right: 20px;
    width: 300px;
    max-height: 400px;
    box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
    transition: opacity 0.3s ease;
}
