import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';
import PropagateLoader from 'react-spinners/PropagateLoader';
import QuestionBlock from './QuestionBlock';

const MainContent = ({ activeTopic }) => {
  const [contentData, setContentData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [selectedQuestion, setSelectedQuestion] = useState(null); // Track the selected question

  const topicQuestions = {
    1: [
      'What are the average class sizes and student-teacher ratios in the local schools?',
      'What are the admission criteria for the schools in this area?',
    ],
    2: ['Topic 2 Question 1', 'Topic 2 Question 2'],
    // Add more topics as needed...
  };

  const fetchDataForQuestion = async (question) => {
    try {
      const response = await axios.post(
        'dummy',
        { question: `${question}:- hi` },
        { headers: { 'Content-Type': 'application/json' } }
      );

      const parsedResponse = JSON.parse(response.data.body);
      const llmAnswer = parsedResponse.answer;
      const formattedAnswer = llmAnswer.split('-').map(line => line.trim()).filter(line => line);

      return formattedAnswer.length > 0 ? formattedAnswer : ['No Answer Available'];
    } catch (error) {
      console.error('Error fetching data:', error);
      return ['No Answer Available'];
    }
  };

  const fetchAllData = async (topicId) => {
    setLoading(true);
    try {
      const questionsList = topicQuestions[topicId] || [];
      const formattedData = await Promise.all(
        questionsList.map(async (question) => {
          const answer = await fetchDataForQuestion(question);
          return {
            question,
            answer: {
              textualResponse: answer,
              citizenExperience: 'Citizen experience response goes here.',
              factualInfo: 'Factual information goes here.',
              contextual: 'Contextual information goes here.',
            },
          };
        })
      );

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

  const handleQuestionClick = (index) => {
    setSelectedQuestion(contentData[index]); // Set the selected question
  };

  return (
    <div className={styles.mainContent}>
      {loading ? (
        <div className={styles.loaderWrapper}>
          <PropagateLoader color="rgb(15, 95, 220)" loading={loading} size={22} />
        </div>
      ) : selectedQuestion ? (
        // Render selected question's data in full screen
        <div className={styles.fullScreenContent}>
          <h2>{selectedQuestion.question}</h2>
          <div>{selectedQuestion.answer.textualResponse.join(', ')}</div>
          {/* Add additional data display as needed */}
          <button onClick={() => setSelectedQuestion(null)}>Back</button>
        </div>
      ) : contentData.length > 0 ? (
        contentData.map((item, index) => (
          <QuestionBlock
            key={index}
            question={item.question}
            answerData={item.answer}
            toggle={() => handleQuestionClick(index)} // Change toggle to handleQuestionClick
          />
        ))
      ) : (
        <div>No Questions Available</div>
      )}
    </div>
  );
};

export default MainContent;



import React from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';
import styles from './QuestionBlock.module.css';
import GridAnswer from './GridAnswer';

const QuestionBlock = ({ question, answerData, isOpen, toggle }) => (
  <div className={styles.questionBlock}>
    <div className={styles.question} onClick={toggle}>
      {question}
      <FontAwesomeIcon icon={isOpen ? faChevronUp : faChevronDown} className={styles.chevronIcon} />
    </div>
    {isOpen && <GridAnswer answerData={answerData} />}
  </div>
);

export default QuestionBlock;




.fullScreenContent {
  position: fixed; /* Position it over the existing content */
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ffffff; /* White background for contrast */
  padding: 20px; /* Padding around the content */
  z-index: 100; /* Ensure it appears above other content */
  overflow-y: auto; /* Allow scrolling if content is large */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); /* Optional shadow */
}

.fullScreenContent h2 {
  margin-top: 0; /* Remove margin for header */
}
