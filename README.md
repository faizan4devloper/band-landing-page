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

  const toggleAnswer = (index) => setOpenQuestion(prevIndex => (prevIndex === index ? null : index));
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


.dropdown {
  position: relative;
  display: inline-block;
  width: 100%;
}

.dropdownButton {
  padding: 10px 20px;
  margin-bottom: 15px;
  background-color: #0073e6;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  transition: background-color 0.3s ease;
}

.dropdownButton:hover {
  background-color: #005bb5;
}

.dropdownIcon {
  margin-left: 8px;
}

.dropdownContent {
  margin-top: 10px;
  padding: 10px;
  background-color: #ffffff;
  border: 1px solid #ddd;
  border-radius: 5px;
  box-shadow: 0px 8px 16px rgba(0, 0, 0, 0.1);
  max-height: 300px;
  overflow-y: auto;
  transition: max-height 0.3s ease;
}

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


.loaderWrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 60vh;
}



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



.questionBlock {
  margin: 0; /* Remove bottom margin to avoid space between dropdown items */
  border: none; /* Remove border for a smoother look */
  background-color: transparent; /* Make the background transparent to blend in with dropdown */
  transition: background-color 0.3s ease, box-shadow 0.3s ease;
}

.questionBlock:hover {
  background-color: #f0f0f0; /* Slightly darken on hover */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Add a subtle shadow on hover */
}

.question {
  font-size: 1rem; /* Maintain same font size */
  font-weight: bold; /* Bold for emphasis */
  padding: 10px 14px; /* Adjust padding for a comfortable click area */
  cursor: pointer; /* Pointer cursor to indicate interactivity */
  color: #2a2a2a; /* Dark text color for visibility */
  display: flex; /* Flexbox for alignment */
  justify-content: space-between; /* Space between question text and chevron */
  align-items: center; /* Center items vertically */
  transition: color 0.3s ease; /* Smooth color transition */
}

.question:hover {
  color: #0073e6; /* Change color on hover */
}

.chevronIcon {
  font-size: 1rem; /* Maintain size */
  color: #888; /* Icon color */
  margin-left: 10px; /* Space from question text */
  transition: transform 0.3s ease, color 0.3s ease; /* Smooth transitions for icon */
}

/* Add additional styling for the expanded answer if needed */
.gridAnswer {
  padding: 10px; /* Padding around the answer content */
  background-color: #fafafa; /* Light background for answers */
  border-top: 1px solid #ddd; /* Subtle border to separate question and answer */
}


