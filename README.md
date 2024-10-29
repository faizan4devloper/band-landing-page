import React, { useState, useEffect, useRef } from 'react';
import axios from 'axios';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faWandSparkles, faUser, faComments } from '@fortawesome/free-solid-svg-icons';
import { BeatLoader } from 'react-spinners';
import styles from './Chatbot.module.css';

const Chatbot = () => {
  const [input, setInput] = useState('');
  const [messages, setMessages] = useState([]);
  const [loading, setLoading] = useState(false);
  const [isChatVisible, setChatVisible] = useState(false); // State for visibility

  const messagesEndRef = useRef(null);

  useEffect(() => {
    // Auto-scroll to the latest message
    if (messagesEndRef.current) {
      messagesEndRef.current.scrollIntoView({ behavior: 'smooth' });
    }
  }, [messages]);

  const handleSubmit = async (e) => {
    e.preventDefault();
    if (input.trim() === '') return;

    const userMessage = { type: 'user', text: input };
    setMessages((prevMessages) => [...prevMessages, userMessage]);
    setInput('');
    setLoading(true);

    try {
      const response = await axios.post('dummy', {
        question: input,
      }, {
        headers: {
          'Content-Type': 'application/json',
        },
      });

      const parsedBody = JSON.parse(response.data.body);
      const answer = parsedBody.answer || 'No answer available for this question.';
      const source = parsedBody.source || 'No source available';

      const botMessage = {
        type: 'bot',
        text: `${answer} (Source: ${source})`,
      };
      setMessages((prevMessages) => [...prevMessages, botMessage]);

    } catch (error) {
      console.error('Error fetching data:', error);
      const errorMessage = { type: 'bot', text: 'Something went wrong. Please try again later.' };
      setMessages((prevMessages) => [...prevMessages, errorMessage]);
    } finally {
      setLoading(false);
    }
  };

  const toggleChatVisibility = () => {
    setChatVisible(!isChatVisible); // Toggle visibility
  };

  return (
    <div className={styles.chatContainer}>
      {/* Conversation Icon */}
      <div className={styles.iconContainer} onClick={toggleChatVisibility}>
        <FontAwesomeIcon icon={faComments} className={styles.conversationIcon} />
      </div>

      {/* Chat Window - Modal Style */}
      {isChatVisible && (
        <div className={styles.chatWindow}>
          <div className={styles.chatHeader}>
            <h4>Chatbot</h4>
            <button className={styles.closeButton} onClick={toggleChatVisibility}>X</button>
          </div>
          <div className={styles.messages}>
            {messages.map((message, index) => (
              <div
                key={index}
                className={message.type === 'user' ? styles.userMessage : styles.botMessage}
              >
                <FontAwesomeIcon
                  icon={message.type === 'user' ? faUser : faWandSparkles}
                  className={styles.icon}
                />
                <div className={styles.messageText}>
                  {message.text}
                </div>
              </div>
            ))}
            {loading && (
              <div className={styles.botMessage}>
                <FontAwesomeIcon icon={faWandSparkles} className={styles.icon} />
                <div className={styles.messageText}>
                  <BeatLoader color="#5f1ec1" size={8} />
                </div>
              </div>
            )}
            <div ref={messagesEndRef} />
          </div>

          {/* Input Form */}
          <form onSubmit={handleSubmit} className={styles.inputForm}>
            <input
              type="text"
              value={input}
              onChange={(e) => setInput(e.target.value)}
              placeholder="Ask a question..."
              className={styles.inputField}
              disabled={loading}
            />
            <button type="submit" className={styles.submitButton} title="Send">
              <FontAwesomeIcon icon={faPaperPlane} />
            </button>
          </form>
        </div>
      )}
    </div>
  );
};

export default Chatbot;



.chatContainer {
  position: relative; /* Required for absolute positioning of chat window */
}

.iconContainer {
  position: fixed; /* Position it on the screen */
  bottom: 20px; /* Adjust as needed */
  right: 20px; /* Adjust as needed */
  background-color: #5f1ec1; /* Background color for the icon */
  border-radius: 50%; /* Circular shape */
  padding: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
  cursor: pointer; /* Change cursor on hover */
  z-index: 1000; /* Ensure it stays on top */
  transition: background-color 0.3s; /* Transition for hover effect */
}

.conversationIcon {
  color: white; /* Icon color */
  font-size: 24px; /* Icon size */
}

.iconContainer:hover {
  background-color: #4a1a90; /* Darken the background on hover */
}

.chatWindow {
  position: fixed; /* Fixed position for modal effect */
  bottom: 80px; /* Adjust position above the icon */
  right: 20px; /* Same as icon */
  width: 300px; /* Fixed width for small screen */
  max-height: 400px; /* Maximum height */
  background-color: white; /* Background color of the chat window */
  border-radius: 10px; /* Rounded corners */
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
  display: flex;
  flex-direction: column; /* Column layout */
  z-index: 1001; /* Above icon */
  overflow: hidden; /* Hide overflow */
}

.chatHeader {
  background-color: #5f1ec1; /* Header color */
  color: white; /* Text color */
  padding: 10px; /* Padding */
  display: flex; /* Flex layout */
  justify-content: space-between; /* Space between elements */
  align-items: center; /* Center items */
}

.closeButton {
  background: none; /* No background */
  border: none; /* No border */
  color: white; /* Text color */
  cursor: pointer; /* Pointer cursor */
}

.messages {
  flex: 1; /* Take up remaining space */
  padding: 10px; /* Padding */
  overflow-y: auto; /* Scrollable messages */
}

.userMessage {
  background-color: #e1ffc7; /* User message background */
  border-radius: 10px; /* Rounded corners */
  margin: 5px 0; /* Margin */
  padding: 10px; /* Padding */
  display: flex; /* Flex layout */
}

.botMessage {
  background-color: #f1f1f1; /* Bot message background */
  border-radius: 10px; /* Rounded corners */
  margin: 5px 0; /* Margin */
  padding: 10px; /* Padding */
  display: flex; /* Flex layout */
}

.messageText {
  margin-left: 10px; /* Spacing for text */
  flex: 1; /* Take up remaining space */
}

.inputForm {
  display: flex; /* Flex layout */
  padding: 10px; /* Padding */
}

.inputField {
  flex: 1; /* Take up remaining space */
  padding: 10px; /* Padding */
  border: 1px solid #ccc; /* Border */
  border-radius: 5px; /* Rounded corners */
}

.submitButton {
  background-color: #5f1ec1; /* Button color */
  color: white; /* Text color */
  border: none; /* No border */
  padding: 10px; /* Padding */
  border-radius: 5px; /* Rounded corners */
  cursor: pointer; /* Pointer cursor */
  margin-left: 10px; /* Spacing */
}










import React, { useState, useEffect, useRef } from 'react';
import axios from 'axios';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faWandSparkles, faUser, faComments } from '@fortawesome/free-solid-svg-icons';
import { BeatLoader } from 'react-spinners';
import styles from './Chatbot.module.css';

const Chatbot = () => {
  const [input, setInput] = useState('');
  const [messages, setMessages] = useState([]);
  const [loading, setLoading] = useState(false);
  const [isChatVisible, setChatVisible] = useState(false); // State for visibility

  const messagesEndRef = useRef(null);

  useEffect(() => {
    // Auto-scroll to the latest message
    if (messagesEndRef.current) {
      messagesEndRef.current.scrollIntoView({ behavior: 'smooth' });
    }
  }, [messages]);

  const handleSubmit = async (e) => {
    e.preventDefault();
    if (input.trim() === '') return;

    const userMessage = { type: 'user', text: input };
    setMessages([...messages, userMessage]);
    setInput('');
    setLoading(true);

    try {
      const response = await axios.post('dummy', {
        question: input,
      }, {
        headers: {
          'Content-Type': 'application/json',
        },
      });

      const parsedBody = JSON.parse(response.data.body);
      const answer = parsedBody.answer || 'No answer available for this question.';
      const source = parsedBody.source || 'No source available';

      const botMessage = {
        type: 'bot',
        text: `${answer} (Source: ${source})`,
      };
      setMessages((prevMessages) => [...prevMessages, botMessage]);

    } catch (error) {
      console.error('Error fetching data:', error);
      const errorMessage = { type: 'bot', text: 'Something went wrong. Please try again later.' };
      setMessages((prevMessages) => [...prevMessages, errorMessage]);
    } finally {
      setLoading(false);
    }
  };

  const toggleChatVisibility = () => {
    setChatVisible(!isChatVisible); // Toggle visibility
  };

  return (
    <div className={styles.chatContainer}>
      {/* Conversation Icon */}
      <div className={styles.iconContainer} onClick={toggleChatVisibility}>
        <FontAwesomeIcon icon={faComments} className={styles.conversationIcon} />
      </div>

      {/* Chat Window */}
      {isChatVisible && (
        <div className={styles.chatWindow}>
          {messages.map((message, index) => (
            <div
              key={index}
              className={message.type === 'user' ? styles.userMessage : styles.botMessage}
            >
              <FontAwesomeIcon
                icon={message.type === 'user' ? faUser : faWandSparkles}
                className={styles.icon}
              />
              <div className={styles.messageText}>
                {message.text}
              </div>
            </div>
          ))}
          {loading && (
            <div className={styles.botMessage}>
              <FontAwesomeIcon icon={faWandSparkles} className={styles.icon} />
              <div className={styles.messageText}>
                <BeatLoader color="#5f1ec1" size={8} />
              </div>
            </div>
          )}
          <div ref={messagesEndRef} />
        </div>
      )}

      {/* Input Form */}
      {isChatVisible && (
        <form onSubmit={handleSubmit} className={styles.inputForm}>
          <input
            type="text"
            value={input}
            onChange={(e) => setInput(e.target.value)}
            placeholder="Ask a question..."
            className={styles.inputField}
            disabled={loading}
          />
          <button type="submit" className={styles.submitButton} title="Send">
            <FontAwesomeIcon icon={faPaperPlane} />
          </button>
        </form>
      )}
    </div>
  );
};

export default Chatbot;



.iconContainer {
  position: fixed; /* Position it on the screen */
  bottom: 20px; /* Adjust as needed */
  right: 20px; /* Adjust as needed */
  background-color: #5f1ec1; /* Background color for the icon */
  border-radius: 50%; /* Circular shape */
  padding: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
  cursor: pointer; /* Change cursor on hover */
  z-index: 1000; /* Ensure it stays on top */
  transition: background-color 0.3s; /* Transition for hover effect */
}

.conversationIcon {
  color: white; /* Icon color */
  font-size: 24px; /* Icon size */
}

.iconContainer:hover {
  background-color: #4a1a90; /* Darken the background on hover */
}
