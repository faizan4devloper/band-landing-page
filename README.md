import React from 'react';
import styles from './QuestionBlock.module.css';

const QuestionBlock = ({ question, answerData, onReset }) => (
  <div className={styles.questionBlock}>
    <div className={styles.question}>
      {question}
    </div>
    <div className={styles.answer}>
      <p><strong>Textual Response:</strong> {answerData.textualResponse.join(', ')}</p>
      <p><strong>Citizen Experience:</strong> {answerData.citizenExperience}</p>
      <p><strong>Factual Info:</strong> {answerData.factualInfo}</p>
      <p><strong>Contextual:</strong> {answerData.contextual}</p>
    </div>
    <button className={styles.resetButton} onClick={onReset}>
      Choose Different Question
    </button>
  </div>
);

export default QuestionBlock;



import React, { useState, useEffect } from 'react';
import axios from 'axios';
import PropagateLoader from 'react-spinners/PropagateLoader';
import FaqDropdown from './FaqDropdown';
import QuestionBlock from './QuestionBlock';
import styles from './MainContent.module.css';

const MainContent = ({ activeTopic }) => {
  const [contentData, setContentData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [selectedQuestion, setSelectedQuestion] = useState(null);
  const [selectedAnswer, setSelectedAnswer] = useState(null);

  const topicQuestions = {
    1: [
      'What are the average class sizes and student-teacher ratios in the local schools?',
      'What are the admission criteria for the schools in this area?',
    ],
    2: ['Topic 2 Question 1', 'Topic 2 Question 2'],
    // Add more topics as needed...
  };

  const fetchDataForQuestion = async (question) => {
    // ... existing code for fetching question data
  };

  const fetchAllData = async (topicId) => {
    // ... existing code for fetching all data
  };

  const handleQuestionSelect = async (question) => {
    // ... existing code for handling question selection
  };

  const handleResetSelection = () => {
    setSelectedQuestion(null);
    setSelectedAnswer(null);
  };

  useEffect(() => {
    fetchAllData(activeTopic);
  }, [activeTopic]);

  return (
    <div className={styles.mainContent}>
      {loading ? (
        <div className={styles.loaderWrapper}>
          <PropagateLoader color="rgb(15, 95, 220)" loading={loading} size={22} />
        </div>
      ) : (
        <>
          {selectedQuestion ? (
            <QuestionBlock
              question={selectedQuestion}
              answerData={selectedAnswer}
              onReset={handleResetSelection} // Pass the reset function
            />
          ) : (
            <FaqDropdown contentData={contentData} onQuestionSelect={handleQuestionSelect} />
          )}
        </>
      )}
    </div>
  );
};

export default MainContent;
