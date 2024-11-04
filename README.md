import React, { useState } from 'react';
import Sidebar from './Sidebar';
import MainContent from './MainContent';
import Chatbot from './Chatbot';

import styles from './EducationPage.module.css';

const EducationPage = () => {
  const [activeTopic, setActiveTopic] = useState(1);
  const [selectedQuestion, setSelectedQuestion] = useState(null);
  const [selectedAnswer, setSelectedAnswer] = useState(null);

  // Function to update the selected question and answer
  const handleQuestionSelect = (question, answer) => {
    setSelectedQuestion(question);
    setSelectedAnswer(answer);
  };

  return (
    <div className={styles.educationPage}>
      <Sidebar setActiveTopic={setActiveTopic} />
      <MainContent
        activeTopic={activeTopic}
        onQuestionSelect={handleQuestionSelect} // Pass handler to MainContent
      />
      <Chatbot
        initialQuestion={selectedQuestion} // Pass selected question to Chatbot
        initialAnswer={selectedAnswer} // Pass selected answer to Chatbot
      />
    </div>
  );
};

export default EducationPage;





import React, { useState, useEffect } from 'react';
import axios from 'axios';
import PropagateLoader from 'react-spinners/PropagateLoader';
import FaqDropdown from './FaqDropdown';
import QuestionBlock from './QuestionBlock';
import styles from './MainContent.module.css';

const MainContent = ({ activeTopic, onQuestionSelect }) => {
  const [contentData, setContentData] = useState([]);
  const [loading, setLoading] = useState(true);

  // Define the questions for each topic
  const topicQuestions = {
    1: [
      { id: 'q1', text: 'What are the last five years key statistics for Serpell Primary School?' },
      { id: 'q2', text: 'What are the admission criteria and process for Serpell Primary School?' },
      { id: 'q3', text: 'How does the school perform in standardized tests and assessments?' },
    ],
    // Add more topics as needed
  };

  const fetchDataForQuestion = async (question) => {
    try {
      const response = await axios.post(
        'dummy', // Replace with your actual endpoint
        { questionId: question.id, questionText: question.text },
        { headers: { 'Content-Type': 'application/json' } }
      );
      const parsedResponse = JSON.parse(response.data.body);
      return {
        question: question.text,
        answer: parsedResponse.answer || 'No answer available.',
      };
    } catch (error) {
      console.error('Error fetching data:', error);
      return { question: question.text, answer: 'Error fetching answer.' };
    }
  };

  const fetchAllData = async (topicId) => {
    setLoading(true);
    try {
      const questionsList = topicQuestions[topicId] || [];
      const formattedData = await Promise.all(questionsList.map(fetchDataForQuestion));
      setContentData(formattedData);
      setLoading(false);
    } catch (error) {
      console.error('Error fetching data for all questions:', error);
      setLoading(false);
    }
  };

  useEffect(() => {
    fetchAllData(activeTopic);
  }, [activeTopic]);

  const handleQuestionClick = (question, answer) => {
    onQuestionSelect(question, answer);
  };

  return (
    <div className={styles.mainContent}>
      {loading ? (
        <div className={styles.loaderWrapper}>
          <PropagateLoader color="rgb(15, 95, 220)" loading={loading} size={22} />
        </div>
      ) : (
        <div>
          {contentData.map((item, index) => (
            <div
              key={index}
              onClick={() => handleQuestionClick(item.question, item.answer)} // Call handler on question click
            >
              <QuestionBlock question={item.question} answerData={item.answer} />
            </div>
          ))}
        </div>
      )}
    </div>
  );
};

export default MainContent;



import React, { useState, useEffect, useRef } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faComments, faTimes } from '@fortawesome/free-solid-svg-icons';
import { BeatLoader } from 'react-spinners';
import styles from './Chatbot.module.css';

const Chatbot = ({ initialQuestion, initialAnswer }) => {
  const [input, setInput] = useState('');
  const [messages, setMessages] = useState([]);
  const [loading, setLoading] = useState(false);
  const [isChatVisible, setChatVisible] = useState(false);

  const messagesEndRef = useRef(null);

  useEffect(() => {
    // Auto-scroll to the latest message
    if (messagesEndRef.current) {
      messagesEndRef.current.scrollIntoView({ behavior: 'smooth' });
    }
  }, [messages]);

  useEffect(() => {
    // Display the selected FAQ when the chatbot opens
    if (initialQuestion && initialAnswer && isChatVisible) {
      setMessages((prevMessages) => [
        ...prevMessages,
        { type: 'bot', text: `Q: ${initialQuestion}` },
        { type: 'bot', text: `A: ${initialAnswer}` },
      ]);
    }
  }, [initialQuestion, initialAnswer, isChatVisible]);

  const handleSubmit = async (e) => {
    e.preventDefault();
    if (!input.trim()) return;

    setMessages((prevMessages) => [...prevMessages, { type: 'user', text: input }]);
    setInput('');
    setLoading(true);

    try {
      // Simulate bot response
      const botMessage = { type: 'bot', text: 'This is a simulated response.' };
      setMessages((prevMessages) => [...prevMessages, botMessage]);
    } catch (error) {
      setMessages((prevMessages) => [...prevMessages, { type: 'bot', text: 'Error fetching answer.' }]);
    } finally {
      setLoading(false);
    }
  };

  const toggleChatVisibility = () => {
    setChatVisible(!isChatVisible);
  };

  return (
    <div className={styles.chatContainer}>
      <div className={styles.iconContainer} onClick={toggleChatVisibility}>
        <FontAwesomeIcon icon={faComments} className={styles.conversationIcon} />
      </div>
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
              <div key={index} className={message.type === 'user' ? styles.userMessage : styles.botMessage}>
                <div className={styles.messageText}>{message.text}</div>
              </div>
            ))}
            {loading && (
              <div className={styles.botMessage}>
                <BeatLoader color="#5f1ec1" size={8} />
              </div>
            )}
            <div ref={messagesEndRef} />
          </div>
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
