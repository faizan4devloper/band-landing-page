// MainContent.js

import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';
import PropagateLoader from 'react-spinners/PropagateLoader';
import QuestionBlock from './QuestionBlock';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';

const MainContent = ({ activeTopic }) => {
  const [contentData, setContentData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [openQuestion, setOpenQuestion] = useState(null);
  const [showFAQDropdown, setShowFAQDropdown] = useState(false);

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

  const toggleAnswer = (index) => {
    setOpenQuestion(prevIndex => (prevIndex === index ? null : index));
    setShowFAQDropdown(false); // Close dropdown after selecting a question
  };
  const toggleFAQDropdown = () => setShowFAQDropdown(prevState => !prevState);

  return (
    <div className={styles.mainContent}>
      {loading ? (
        <div className={styles.loaderWrapper}>
          <PropagateLoader color="rgb(15, 95, 220)" loading={loading} size={22} />
        </div>
      ) : (
        <>
          <div className={styles.dropdown}>
            <button onClick={toggleFAQDropdown} className={styles.dropdownButton}>
              FAQ
              <FontAwesomeIcon icon={showFAQDropdown ? faChevronUp : faChevronDown} className={styles.dropdownIcon} />
            </button>
            {showFAQDropdown && (
              <div className={styles.dropdownContent}>
                {contentData.length > 0 ? (
                  contentData.map((item, index) => (
                    <QuestionBlock
                      key={index}
                      question={item.question}
                      answerData={item.answer}
                      isOpen={openQuestion === index}
                      toggle={() => toggleAnswer(index)}
                      isActive={openQuestion === index} // New prop to mark active question
                    />
                  ))
                ) : (
                  <div>No Questions Available</div>
                )}
              </div>
            )}
          </div>
        </>
      )}
    </div>
  );
};

export default MainContent;



// QuestionBlock.js

import React from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';
import styles from './QuestionBlock.module.css';
import GridAnswer from './GridAnswer';

const QuestionBlock = ({ question, answerData, isOpen, toggle, isActive }) => (
  <div className={`${styles.questionBlock} ${isActive ? styles.active : ''}`}>
    <div className={styles.question} onClick={toggle}>
      {question}
      <FontAwesomeIcon icon={isOpen ? faChevronUp : faChevronDown} className={styles.chevronIcon} />
    </div>
    {isOpen && <GridAnswer answerData={answerData} />}
  </div>
);

export default QuestionBlock;



/* QuestionBlock.module.css */

.questionBlock {
  margin: 0;
  border: none;
  background-color: transparent;
  transition: background-color 0.3s ease, box-shadow 0.3s ease;
}

.questionBlock:hover {
  background-color: #f0f0f0;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.question {
  font-size: 1rem;
  font-weight: bold;
  padding: 10px 14px;
  cursor: pointer;
  color: #2a2a2a;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: color 0.3s ease;
}

.question:hover {
  color: #0073e6;
}

.chevronIcon {
  font-size: 1rem;
  color: #888;
  margin-left: 10px;
  transition: transform 0.3s ease, color 0.3s ease;
}

.active {
  background-color: #e0f3ff;
  border-left: 4px solid #0073e6;
}
