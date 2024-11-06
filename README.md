import React, { useState, useEffect, useRef } from 'react';
import axios from 'axios';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faWandSparkles, faUser, faComments, faTimes } from '@fortawesome/free-solid-svg-icons';
import { BeatLoader } from 'react-spinners';
import styles from './Chatbot.module.css';

const Chatbot = ({ questionFromMain }) => {
  const [input, setInput] = useState('');
  const [messages, setMessages] = useState([]);
  const [loading, setLoading] = useState(false);
  const [isChatVisible, setChatVisible] = useState(false);
  const messagesEndRef = useRef(null);

  useEffect(() => {
    if (messagesEndRef.current) {
      messagesEndRef.current.scrollIntoView({ behavior: 'smooth' });
    }
  }, [messages]);

  useEffect(() => {
    if (questionFromMain) {
      const newMessage = { type: 'user', text: questionFromMain.question };
      setMessages((prev) => [...prev, newMessage]);
      handleQuestionSubmit(questionFromMain.question);
    }
  }, [questionFromMain]);

  const handleQuestionSubmit = async (question) => {
    setLoading(true);
    try {
      const response = await axios.post('dummy', { question });
      const parsedBody = JSON.parse(response.data.body);
      const answer = parsedBody.answer || 'No answer available for this question.';
      setMessages((prev) => [...prev, { type: 'bot', text: answer }]);
    } catch (error) {
      setMessages((prev) => [...prev, { type: 'bot', text: 'Error fetching data' }]);
    } finally {
      setLoading(false);
    }
  };

  return (
    <div className={styles.chatContainer}>
      <div className={styles.iconContainer} onClick={() => setChatVisible(!isChatVisible)}>
        <FontAwesomeIcon icon={faComments} className={styles.conversationIcon} />
      </div>
      {isChatVisible && (
        <div className={styles.chatWindow}>
          <div className={styles.chatHeader}>
            <p>Chatbot</p>
            <button onClick={() => setChatVisible(false)}><FontAwesomeIcon icon={faTimes} /></button>
          </div>
          <div className={styles.messages}>
            {messages.map((message, index) => (
              <div key={index} className={message.type === 'user' ? styles.userMessage : styles.botMessage}>
                <FontAwesomeIcon icon={message.type === 'user' ? faUser : faWandSparkles} />
                <div>{message.text}</div>
              </div>
            ))}
            {loading && <BeatLoader color="#5f1ec1" size={8} />}
            <div ref={messagesEndRef} />
          </div>
          <form onSubmit={(e) => e.preventDefault()}>
            <input
              type="text"
              value={input}
              onChange={(e) => setInput(e.target.value)}
              placeholder="Ask a question..."
              disabled={loading}
            />
            <button type="submit">
              <FontAwesomeIcon icon={faPaperPlane} />
            </button>
          </form>
        </div>
      )}
    </div>
  );
};

export default Chatbot;




import React, { useState, useEffect } from 'react';
import axios from 'axios';
import PropagateLoader from 'react-spinners/PropagateLoader';
import FaqDropdown from './FaqDropdown';
import QuestionBlock from './QuestionBlock';
import styles from './MainContent.module.css';

const MainContent = ({ activeTopic, onQuestionClick }) => {
  const [contentData, setContentData] = useState([]); // Stores fetched data for questions
  const [loading, setLoading] = useState(true); // Loader state
  const [selectedData, setSelectedData] = useState({}); // Stores selected question data

  const topicQuestions = {
    1: [
      { id: 'q1', text: 'What are the last five years key statistics for Serpell Primary School?' },
      { id: 'q2', text: 'What are the admission criteria and process for Serpell Primary School?' },
      { id: 'q3', text: 'How does the school perform in standardized tests and assessments?' },
    ],
    // Other topics...
  };

  const fetchDataForQuestion = async ({ id, text }) => {
    try {
      const response = await axios.post(
        'dummy', // Replace with your actual endpoint
        { questionId: id, questionText: text },
        { headers: { 'Content-Type': 'application/json' } }
      );
      const parsedResponse = JSON.parse(response.data.body);
      const llmAnswer = parsedResponse.answer || '';

      return {
        textualResponse: llmAnswer.split('-').map(line => line.trim()).filter(line => line),
        factualData: parsedResponse.factualData,
        citizenReview: parsedResponse.citizenReview,
        contextual: parsedResponse.contextual || 'No contextual information available.',
      };
    } catch (error) {
      return { textualResponse: ['No Answer Available'] };
    }
  };

  const fetchAllData = async (topicId) => {
    setLoading(true);
    try {
      const questionsList = topicQuestions[topicId] || [];
      const formattedData = await Promise.all(
        questionsList.map(async (question) => {
          const answerData = await fetchDataForQuestion(question);
          return { question: question.text, answer: answerData };
        })
      );

      setContentData(formattedData);
      setLoading(false);
    } catch (error) {
      setLoading(false);
    }
  };

  const handleQuestionSelect = (question, answer) => {
    setSelectedData({ question, answer });
    onQuestionClick(question, answer); // Pass question to chatbot on click
  };

  useEffect(() => {
    fetchAllData(activeTopic);
    setSelectedData({});
  }, [activeTopic]);

  return (
    <div className={styles.mainContent}>
      {loading ? (
        <div className={styles.loaderWrapper}>
          <PropagateLoader color="rgb(15, 95, 220)" loading={loading} size={22} />
        </div>
      ) : (
        <>
          <FaqDropdown contentData={contentData} onQuestionSelect={handleQuestionSelect} />
          {selectedData.question && (
            <QuestionBlock question={selectedData.question} answerData={selectedData.answer} />
          )}
        </>
      )}
    </div>
  );
};

export default MainContent;





import React, { useState } from 'react';
import Sidebar from './Sidebar';
import MainContent from './MainContent';
import Chatbot from './Chatbot';
import styles from './EducationPage.module.css';

const EducationPage = () => {
  const [activeTopic, setActiveTopic] = useState(1);
  const [selectedQuestion, setSelectedQuestion] = useState(null);

  const handleQuestionClick = (question, answer) => {
    setSelectedQuestion({ question, answer });
  };

  return (
    <div className={styles.educationPage}>
      <Sidebar setActiveTopic={setActiveTopic} />
      <MainContent activeTopic={activeTopic} onQuestionClick={handleQuestionClick} />
      <Chatbot questionFromMain={selectedQuestion} />
    </div>
  );
};

export default EducationPage;
