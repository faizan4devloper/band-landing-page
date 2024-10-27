import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';
import PropagateLoader from 'react-spinners/PropagateLoader';
import QuestionBlock from './QuestionBlock';

const MainContent = ({ activeTopic }) => {
  const [contentData, setContentData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [selectedQuestion, setSelectedQuestion] = useState(null); // To store the selected question

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

  const handleQuestionSelect = (item) => {
    setSelectedQuestion(item); // Set the selected question
  };

  return (
    <div className={styles.mainContent}>
      {loading ? (
        <div className={styles.loaderWrapper}>
          <PropagateLoader color="rgb(15, 95, 220)" loading={loading} size={22} />
        </div>
      ) : contentData.length > 0 ? (
        contentData.map((item, index) => (
          <QuestionBlock
            key={index}
            question={item.question}
            answerData={item.answer}
            onSelect={() => handleQuestionSelect(item)} // Pass the item to the select handler
          />
        ))
      ) : (
        <div>No Questions Available</div>
      )}

      {/* Render selected question data if exists */}
      {selectedQuestion && (
        <div className={styles.selectedQuestionContainer}>
          <h2>{selectedQuestion.question}</h2>
          <div>{selectedQuestion.answer.textualResponse.join(', ')}</div>
          {/* Add more answer data rendering as needed */}
          <button onClick={() => setSelectedQuestion(null)}>Back to Questions</button>
        </div>
      )}
    </div>
  );
};

export default MainContent;




import React from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';
import styles from './QuestionBlock.module.css';

const QuestionBlock = ({ question, answerData, onSelect }) => (
  <div className={styles.questionBlock} onClick={onSelect}>
    <div className={styles.question}>
      {question}
      <FontAwesomeIcon icon={faChevronDown} className={styles.chevronIcon} />
    </div>
  </div>
);

export default QuestionBlock;



.selectedQuestionContainer {
  position: fixed; /* Cover the full screen */
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(255, 255, 255, 0.9); /* Slightly transparent background */
  padding: 20px;
  overflow-y: auto; /* Allow scrolling if content exceeds screen */
  z-index: 1000; /* Ensure it’s on top of other content */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.selectedQuestionContainer h2 {
  margin-top: 0; /* Remove margin from the top */
}
