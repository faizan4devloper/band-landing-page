import React, { useState, useEffect } from 'react';
import axios from 'axios';
import PropagateLoader from 'react-spinners/PropagateLoader';
import FaqDropdown from './FaqDropdown';
import styles from './MainContent.module.css';

const MainContent = ({ activeTopic }) => {
  const [contentData, setContentData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [selectedQuestionIndex, setSelectedQuestionIndex] = useState(null);

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

  const handleQuestionSelect = (index) => {
    setSelectedQuestionIndex(index);
  };

  return (
    <div className={styles.mainContent}>
      {loading ? (
        <div className={styles.loaderWrapper}>
          <PropagateLoader color="rgb(15, 95, 220)" loading={loading} size={22} />
        </div>
      ) : (
        <div>
          <FaqDropdown
            contentData={contentData}
            onQuestionSelect={handleQuestionSelect}
          />
          {selectedQuestionIndex !== null && (
            <div className={styles.selectedAnswer}>
              <h3>Selected Question Answer:</h3>
              <QuestionBlock
                question={contentData[selectedQuestionIndex].question}
                answerData={contentData[selectedQuestionIndex].answer}
              />
            </div>
          )}
        </div>
      )}
    </div>
  );
};

export default MainContent;


import React, { useState } from 'react';
import QuestionBlock from './QuestionBlock';
import styles from './FaqDropdown.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';

const FaqDropdown = ({ contentData, onQuestionSelect }) => {
  const [isFaqOpen, setIsFaqOpen] = useState(false);
  
  const toggleFaq = () => setIsFaqOpen(!isFaqOpen);

  const handleQuestionClick = (index) => {
    onQuestionSelect(index); // Pass the selected question index
    setIsFaqOpen(false); // Close the dropdown
  };

  return (
    <div className={styles.faqDropdown}>
      <div className={styles.dropdownHeader} onClick={toggleFaq}>
        <span>Frequently Asked Questions</span>
        <FontAwesomeIcon icon={isFaqOpen ? faChevronUp : faChevronDown} />
      </div>
      {isFaqOpen && (
        <div className={styles.dropdownContent}>
          {contentData.map((item, index) => (
            <div key={index} onClick={() => handleQuestionClick(index)}>
              <QuestionBlock
                question={item.question}
                answerData={item.answer}
                isActive={item.isActive} // Check if the question is active
              />
            </div>
          ))}
        </div>
      )}
    </div>
  );
};

export default FaqDropdown;


import React from 'react';
import styles from './QuestionBlock.module.css';

const QuestionBlock = ({ question, answerData, isActive }) => (
  <div className={`${styles.questionBlock} ${isActive ? styles.active : ''}`}>
    <div className={styles.question}>
      {question}
    </div>
    {isActive && (
      <div className={styles.answer}>
        <p><strong>Textual Response:</strong> {answerData.textualResponse.join(', ')}</p>
        <p><strong>Citizen Experience:</strong> {answerData.citizenExperience}</p>
        <p><strong>Factual Info:</strong> {answerData.factualInfo}</p>
        <p><strong>Contextual:</strong> {answerData.contextual}</p>
      </div>
    )}
  </div>
);

export default QuestionBlock;


.questionBlock {
  padding: 15px;
  border-bottom: 1px solid #ddd;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.questionBlock:hover {
  background-color: #f0f0f0;
}

.questionBlock.active {
  background-color: #0073e6; /* Highlight color */
  color: #fff; /* Change text color when active */
}

.question {
  font-size: 1rem;
  font-weight: bold;
  color: inherit; /* Inherit color from parent */
}

.answer {
  margin-top: 10px;
  padding: 10px;
  background-color: #fafafa;
  border-left: 4px solid #0073e6;
}
