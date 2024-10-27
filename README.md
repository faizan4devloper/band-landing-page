import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';
import PropagateLoader from 'react-spinners/PropagateLoader';
import QuestionBlock from './QuestionBlock';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown } from '@fortawesome/free-solid-svg-icons';

const MainContent = ({ activeTopic }) => {
  const [contentData, setContentData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [activeQuestion, setActiveQuestion] = useState(null);

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

  const handleQuestionClick = (index) => setActiveQuestion(prevIndex => (prevIndex === index ? null : index));

  return (
    <div className={styles.mainContent}>
      {loading ? (
        <div className={styles.loaderWrapper}>
          <PropagateLoader color="rgb(15, 95, 220)" loading={loading} size={22} />
        </div>
      ) : (
        <>
          <div className={styles.questionList}>
            {contentData.length > 0 ? (
              contentData.map((item, index) => (
                <QuestionBlock
                  key={index}
                  question={item.question}
                  answerData={item.answer}
                  isOpen={activeQuestion === index}
                  toggle={() => handleQuestionClick(index)}
                />
              ))
            ) : (
              <div>No Questions Available</div>
            )}
          </div>
          <button onClick={() => setActiveQuestion(null)} className={styles.faqButton}>
            FAQ
            <FontAwesomeIcon icon={faChevronDown} className={styles.faqIcon} />
          </button>
        </>
      )}
    </div>
  );
};

export default MainContent;



.mainContent {
  flex-grow: 1;
  height: 100vh;
  padding: 35px;
  padding-top: 60px;
  background-color: #f7f9fc;
  overflow-y: auto;
  width: 800px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
}

.questionList {
  flex-grow: 1;
}

.faqButton {
  margin-top: auto;
  padding: 10px 20px;
  background-color: #0073e6;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  transition: background-color 0.3s ease;
}

.faqButton:hover {
  background-color: #005bb5;
}

.faqIcon {
  margin-left: 8px;
}

.loaderWrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 60vh;
}




import React from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown } from '@fortawesome/free-solid-svg-icons';
import styles from './QuestionBlock.module.css';
import GridAnswer from './GridAnswer';

const QuestionBlock = ({ question, answerData, isOpen, toggle }) => (
  <div
    className={`${styles.questionBlock} ${isOpen ? styles.active : ''}`}
    onClick={toggle}
  >
    <div className={styles.question}>
      {question}
      <FontAwesomeIcon icon={faChevronDown} className={styles.chevronIcon} />
    </div>
    {isOpen && <GridAnswer answerData={answerData} />}
  </div>
);

export default QuestionBlock;



.questionBlock {
  margin: 0;
  background-color: transparent;
  transition: background-color 0.3s ease;
}

.questionBlock.active {
  background-color: #f0f8ff; /* Light blue for active state */
}

.questionBlock:hover {
  background-color: #f0f0f0;
}

.question {
  font-weight: bold;
  padding: 10px 14px;
  cursor: pointer;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: color 0.3s ease;
}

.question:hover {
  color: #0073e6;
}

.chevronIcon {
  color: #888;
  transition: transform 0.3s ease;
}

.gridAnswer {
  padding: 10px;
  background-color: #fafafa;
  border-top: 1px solid #ddd;
}

