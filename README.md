import React, { useState } from 'react';
import Sidebar from './Sidebar';
import MainContent from './MainContent';
import Chatbot from './Chatbot';
import styles from './EducationPage.module.css';

const EducationPage = () => {
  const [activeTopic, setActiveTopic] = useState(1);
  const [messages, setMessages] = useState([]);

  const addMessageToChatbot = (question, response) => {
    setMessages((prevMessages) => [
      ...prevMessages,
      { type: 'user', text: question },
      { type: 'bot', text: response }
    ]);
  };

  return (
    <div className={styles.educationPage}>
      <Sidebar setActiveTopic={setActiveTopic} />
      <MainContent activeTopic={activeTopic} addMessageToChatbot={addMessageToChatbot} />
      <Chatbot initialMessages={messages} />
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

const MainContent = ({ activeTopic, addMessageToChatbot }) => {
  const [contentData, setContentData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [selectedData, setSelectedData] = useState({});

  // Define the questions for each topic
  const topicQuestions = {
    1: [
      { id: 'q1', text: 'What are the last five years key statistics for Serpell Primary School?' },
      { id: 'q2', text: 'What are the admission criteria and process for Serpell Primary School?' },
      { id: 'q3', text: 'How does the school perform in standardized tests and assessments?' },
    ],
    // ... other topics
  };

  const fetchDataForQuestion = async ({ id, text }) => {
    // Fetching data logic here
  };

  const fetchAllData = async (topicId) => {
    // Fetching data for all questions logic here
  };

  const handleQuestionSelect = (question, answer) => {
    addMessageToChatbot(question, answer.textualResponse.join(' '));
    setSelectedData((prev) => ({
      ...prev,
      [activeTopic]: {
        question,
        answer,
      },
    }));
  };

  useEffect(() => {
    fetchAllData(activeTopic);
    setSelectedData((prev) => ({ ...prev, [activeTopic]: null }));
  }, [activeTopic]);

  const selectedQuestionData = selectedData[activeTopic] || {};

  return (
    <div className={styles.mainContent}>
      {loading ? (
        <div className={styles.loaderWrapper}>
          <PropagateLoader color="rgb(15, 95, 220)" loading={loading} size={22} />
        </div>
      ) : (
        <>
          <FaqDropdown
            contentData={contentData}
            onQuestionSelect={handleQuestionSelect}
            selectedQuestion={selectedQuestionData.question}
            selectedAnswer={selectedQuestionData.answer}
          />
          {selectedQuestionData.question && selectedQuestionData.answer && (
            <div className={styles.selectedQuestionBlock}>
              <QuestionBlock
                question={selectedQuestionData.question}
                answerData={selectedQuestionData.answer}
                onClick={() => handleQuestionSelect(selectedQuestionData.question, selectedQuestionData.answer)}
              />
            </div>
          )}
        </>
      )}
    </div>
  );
};

export default MainContent;


import React from 'react';
import styles from './QuestionBlock.module.css';

const QuestionBlock = ({ question, answerData, onClick }) => {
  return (
    <div className={styles.questionBlock}>
      <div className={styles.question}>{question}</div>

      <div className={styles.responseSection} onClick={onClick}>
        <p><strong>Textual Response:</strong></p>
        <ul>
          {answerData.textualResponse.map((item, index) => (
            <li key={index}>{item}</li>
          ))}
        </ul>
      </div>
      {/* More response sections */}
    </div>
  );
};

export default QuestionBlock;



import React, { useState, useEffect, useRef } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faWandSparkles, faUser, faComments, faTimes } from '@fortawesome/free-solid-svg-icons';
import { BeatLoader } from 'react-spinners';
import styles from './Chatbot.module.css';

const Chatbot = ({ initialMessages = [] }) => {
  const [messages, setMessages] = useState(initialMessages);
  const [input, setInput] = useState('');
  const [loading, setLoading] = useState(false);
  const [isChatVisible, setChatVisible] = useState(false);

  const messagesEndRef = useRef(null);

  useEffect(() => {
    if (messagesEndRef.current) {
      messagesEndRef.current.scrollIntoView({ behavior: 'smooth' });
    }
  }, [messages]);

  useEffect(() => {
    setMessages(initialMessages);
  }, [initialMessages]);

  const handleSubmit = async (e) => {
    e.preventDefault();
    if (input.trim() === '') return;

    const userMessage = { type: 'user', text: input };
    setMessages((prevMessages) => [...prevMessages, userMessage]);
    setInput('');
    setLoading(true);

    // Assume fetching response logic
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
              <div
                key={index}
                className={message.type === 'user' ? styles.userMessage : styles.botMessage}
              >
                <FontAwesomeIcon icon={message.type === 'user' ? faUser : faWandSparkles} />
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
              disabled={loading}
              className={styles.inputField}
            />
            <button type="submit" className={styles.submitButton}>
              <FontAwesomeIcon icon={faPaperPlane} />
            </button>
          </form>
        </div>
      )}
    </div>
  );
};

export default Chatbot;

