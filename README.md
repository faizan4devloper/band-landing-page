:root {
  --primary-color: #5f1ec1;
  --secondary-color: rgba(15, 95, 220, 1);
  --background-color: #ffffff;
  --chat-text-color: #fff;
  --message-background: #f1f1f1;
  --user-message-background: #d1e7ff;
  --input-background: #ffffff;
  --input-border: #ddd;
  --loading-color: #5f1ec1;
}

/* Dark Theme */
[data-theme="dark"] {
  --primary-color: #9d66f5;
  --background-color: #1a1a2e;
  --message-background: #333;
  --user-message-background: #444;
  --input-background: #444;
  --input-border: #666;
  --loading-color: #c1a1f2;
}


.chatContainer {
    position: fixed;
    bottom: 20px;
    right: 20px;
    z-index: 1000;
}

.iconContainer {
  cursor: pointer;
  padding: 14px;
  border-radius: 30%;
  background: linear-gradient(90deg, var(--primary-color) 0%, var(--secondary-color) 100%);
  transition: transform 0.3s ease;
}

/* Chat Window - Modal Style */
.chatWindow {
  position: fixed;
  bottom: 80px;
  right: 20px;
  width: 320px;
  height: 420px;
  background-color: var(--background-color);
  border-radius: 8px;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.3);
  display: flex;
  flex-direction: column;
  transition: all 0.3s ease;
}

.chatHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: linear-gradient(90deg, var(--primary-color) 0%, var(--secondary-color) 100%);
  color: var(--chat-text-color);
  padding: 5px;
  border-top-left-radius: 8px;
  border-top-right-radius: 8px;
}

.chatHeading {
  font-size: 14px;
  font-weight: bold;
  text-align: center;
}

.closeButton {
  background: none;
  border: none;
  color: var(--chat-text-color);
  font-size: 16px;
  cursor: pointer;
}

.messages {
  flex: 1;
  padding: 10px;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
}

.userMessage,
.botMessage {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.userMessage {
  justify-content: flex-end;
}

.botMessage {
  justify-content: flex-start;
}

.icon {
  margin: 0 8px;
  font-size: 15px;
  color: #fff;
}

.messageText {
  max-width: 75%;
  background-color: #f1f1f1;
  padding: 10px;
  font-size: 12px;
  border-radius: 10px;
  color: #333;
  word-wrap: break-word;
  white-space: pre-wrap;
}

.userMessage .messageText {
  background-color: #d1e7ff;
}

.inputForm {
  display: flex;
  border-top: 1px solid #ddd;
  padding: 10px;
}

.inputField {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  color: #000;
  outline: none;
}

.submitButton {
  background: linear-gradient(90deg, var(--primary-color) 0%, var(--secondary-color) 100%);
  color: var(--chat-text-color);
  border: none;
  padding: 10px;
  border-radius: 4px;
  margin-left: 10px;
  cursor: pointer;
}

/* Follow-Up Questions */
.followUpQuestions {
  margin-top: 15px;
  padding: 10px;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0 0 8px rgba(0, 0, 0, 0.1);
}

.followUpQuestions p {
  font-weight: bold;
  font-size: 14px;
  margin-bottom: 10px;
  color: #333;
}

.followUpButton {
  display: block;
  margin: 5px 0;
  padding: 10px;
  font-size: 12px;
  background: linear-gradient(90deg, var(--primary-color) 0%, var(--secondary-color) 100%);
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background 0.3s ease;
  text-align: left;
}

.followUpButton:hover {
  background: linear-gradient(90deg, var(--secondary-color) 0%, var(--primary-color) 100%);
}

.followUpButton:focus {
  outline: none;
}

.messages::-webkit-scrollbar {
  width: 6px;
}

.messages::-webkit-scrollbar-thumb {
  background-color: rgba(15, 95, 220, 1);
  border-radius: 10px;
}






import React, { useState, useEffect, useRef } from 'react';
import axios from 'axios';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faWandSparkles, faUser, faComments, faTimes } from '@fortawesome/free-solid-svg-icons';
import { BeatLoader } from 'react-spinners';
import styles from './Chatbot.module.css';

const Chatbot = () => {
  const [input, setInput] = useState('');
  const [messages, setMessages] = useState([]);
  const [loading, setLoading] = useState(false);
  const [isChatVisible, setChatVisible] = useState(false); // State for visibility
  const [currentQuestionId, setCurrentQuestionId] = useState(null); // Track the current question ID
  const [followUpQuestions, setFollowUpQuestions] = useState([]); // Store follow-up questions
  
  const messagesEndRef = useRef(null);

  useEffect(() => {
    // Auto-scroll to the latest message
    if (messagesEndRef.current) {
      messagesEndRef.current.scrollIntoView({ behavior: 'smooth' });
    }
  }, [messages]);

  const generateFollowUpQuestions = (answer) => {
    // Example logic for generating follow-up questions dynamically based on the current answer
    const followUpQuestionsList = [
      `Can you explain more about ${answer.slice(0, 10)}?`,
      `What are the details of ${answer.slice(0, 10)}?`,
      `How does ${answer.slice(0, 10)} affect the outcome?`,
      `Can you give an example related to ${answer.slice(0, 10)}?`,
      `What should I do next after knowing about ${answer.slice(0, 10)}?`
    ];
    setFollowUpQuestions(followUpQuestionsList);
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    if (input.trim() === '') return;

    const userMessage = { type: 'user', text: input };
    setMessages((prevMessages) => [...prevMessages, userMessage]);
    setInput('');
    setLoading(true);

    try {
      // Dynamically set the question_id (e.g., Q1, Q2, etc.)
      const questionId = `Q${Math.floor(Math.random() * 9) + 1}`; // Randomly generates Q1-Q9

      const payload = {
        question: input,
        question_id: questionId,  // Dynamically assigned question_id
      };

      const response = await axios.post('dummy', payload, {
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
      
      // Store the current question ID and generate follow-up questions based on the answer
      setCurrentQuestionId(questionId);
      generateFollowUpQuestions(answer);

    } catch (error) {
      console.error('Error fetching data:', error);
      const errorMessage = { type: 'bot', text: 'Something went wrong. Please try again later.' };
      setMessages((prevMessages) => [...prevMessages, errorMessage]);
    } finally {
      setLoading(false);
    }
  };

  const handleFollowUp = async (followUpQuestion) => {
    const userMessage = { type: 'user', text: followUpQuestion };
    setMessages((prevMessages) => [...prevMessages, userMessage]);
    setLoading(true);

    try {
      // Use the current question ID for the follow-up question
      const payload = {
        question: followUpQuestion,
        question_id: currentQuestionId,  // Use the current question ID for follow-up
      };

      const response = await axios.post('dummy', payload, {
        headers: {
          'Content-Type': 'application/json',
        },
      });

      const parsedBody = JSON.parse(response.data.body);
      const followUpAnswer = parsedBody.answer || 'No answer available for this follow-up.';
      const botMessage = {
        type: 'bot',
        text: followUpAnswer,
      };
      setMessages((prevMessages) => [...prevMessages, botMessage]);
    } catch (error) {
      console.error('Error fetching follow-up data:', error);
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
            <p className={styles.chatHeading}>Chatbot</p>
            <button className={styles.closeButton} onClick={toggleChatVisibility}>
              <FontAwesomeIcon icon={faTimes} />
            </button>
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

          {/* Follow-Up Questions */}
          {followUpQuestions.length > 0 && (
            <div className={styles.followUpQuestions}>
              <p>Follow-up Questions:</p>
              {followUpQuestions.map((question, index) => (
                <button
                  key={index}
                  className={styles.followUpButton}
                  onClick={() => handleFollowUp(question)}
                >
                  {question}
                </button>
              ))}
            </div>
          )}
        </div>
      )}
    </div>
  );
};

export default Chatbot;




:root {
  --primary-color: #5f1ec1;
  --secondary-color: rgba(15, 95, 220, 1);
  --background-color: #ffffff;
  --chat-text-color: #fff;
  --message-background: #f1f1f1;
  --user-message-background: #d1e7ff;
  --input-background: #ffffff;
  --input-border: #ddd;
  --loading-color: #5f1ec1;
}

/* Dark Theme */
[data-theme="dark"] {
  --primary-color: #9d66f5;
  --background-color: #1a1a2e;
  --message-background: #333;
  --user-message-background: #444;
  --input-background: #444;
  --input-border: #666;
  --loading-color: #c1a1f2;
}


.chatContainer {
    position: fixed;
    bottom: 20px;
    color:#fff;
    /*padding: 14px;*/
    /*border-radius: 30%;*/
    /*background: linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%);*/
    right: 20px;
    z-index: 1000;
}

.iconContainer {
  cursor: pointer;  
  padding: 14px;
  border-radius: 30%;
  background: linear-gradient(90deg, var(--primary-color) 0%, var(--secondary-color) 100%);
  transition: transform 0.3s ease;
}

/* Chat Window - Modal Style */
.chatWindow {
  position: fixed;
  bottom: 80px;
  right: 20px;
  width: 320px;
  height: 420px;
  background-color: var(--background-color);
  border-radius: 8px;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.3);
  display: flex;
  flex-direction: column;
  transition: all 0.3s ease;
}

.chatHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: linear-gradient(90deg, var(--primary-color) 0%, var(--secondary-color) 100%);
  color: var(--chat-text-color);
  padding: 5px;
  border-top-left-radius: 8px;
  border-top-right-radius: 8px;
}

.chatHeading{
  font-size: 14px;
  font-weight: bold;
  text-align:center;
}


.closeButton {
  background: none;
  border: none;
  color: var(--chat-text-color);
  font-size: 16px;
  cursor: pointer;
}

.messages {
  flex: 1;
  padding: 10px;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
}

.userMessage,
.botMessage {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.userMessage {
  justify-content: flex-end;
}

.botMessage {
  justify-content: flex-start;
}

.icon {
  margin: 0 8px;
  font-size: 15px;
  color: #fff;
}

.messageText {
  max-width: 75%;
  background-color: #f1f1f1;
  padding: 10px;
  font-size: 12px;
  border-radius: 10px;
  color: #333;
  word-wrap: break-word;
  white-space: pre-wrap;
}

.userMessage .messageText {
  background-color: #d1e7ff;
}

.inputForm {
  display: flex;
  border-top: 1px solid #ddd;
  padding: 10px;
}

.inputField {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  color: #000;
  outline: none;
}

.submitButton {
  background: linear-gradient(90deg, var(--primary-color) 0%, var(--secondary-color) 100%);
  color: var(--chat-text-color);
  border: none;
  padding: 10px;
  border-radius: 4px;
  margin-left: 10px;
  cursor: pointer;
}


.messages::-webkit-scrollbar{
  width:6px;
}
.messages::-webkit-scrollbar-thumb{
background-color: rgba(15, 95, 220, 1);
border-radius: 10px
}


