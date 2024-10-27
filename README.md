import React from 'react';
import styles from './FaqDropdown.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';
import QuestionBlock from './QuestionBlock';

const FaqDropdown = ({ contentData, onQuestionSelect, selectedQuestion, selectedAnswer }) => {
  const [isFaqOpen, setIsFaqOpen] = React.useState(false); // Initially closed

  const toggleFaq = () => setIsFaqOpen(!isFaqOpen);

  const handleQuestionSelect = (question, answer) => {
    onQuestionSelect(question, answer);
    setIsFaqOpen(false); // Close dropdown after selection
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
            <div 
              key={index} 
              onClick={() => handleQuestionSelect(item.question, item.answer)} 
              className={`${styles.questionBlock} ${selectedQuestion === item.question ? styles.activeQuestion : ''}`}
            >
              <span>{item.question}</span>
            </div>
          ))}
        </div>
      )}
    </div>
  );
};

export default FaqDropdown;




import React, { useState, useEffect } from 'react';
import axios from 'axios';
import PropagateLoader from 'react-spinners/PropagateLoader';
import FaqDropdown from './FaqDropdown';
import QuestionBlock from './QuestionBlock'; // Import QuestionBlock here
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

  const handleQuestionSelect = (question, answer) => {
    setSelectedQuestion(question);
    setSelectedAnswer(answer);
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
          <FaqDropdown
            contentData={contentData}
            onQuestionSelect={handleQuestionSelect}
            selectedQuestion={selectedQuestion}
            selectedAnswer={selectedAnswer}
          />
          {selectedQuestion && selectedAnswer && (
            <div className={styles.selectedQuestionBlock}>
              <QuestionBlock
                question={selectedQuestion}
                answerData={selectedAnswer}
              />
            </div>
          )}
        </>
      )}
    </div>
  );
};

export default MainContent;



.faqDropdown {
  border: 1px solid #ddd;
  margin-top: 20px;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Added shadow for depth */
}

.dropdownHeader {
  display: flex;
  justify-content: space-between;
  align-items: center; /* Center align items vertically */
  padding: 15px;
  background-color: #0073e6; /* Changed to a primary color */
  color: white; /* Changed text color to white */
  cursor: pointer;
  font-weight: bold;
  border-radius: 5px 5px 0 0; /* Rounded corners on the top */
}

.dropdownContent {
  padding: 10px 15px;
  border-top: 1px solid #ddd; /* Separate the header from the content */
  background-color: #fff; /* White background for content */
}

.questionBlock {
  padding: 10px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  border-radius: 4px; /* Rounded corners */
}

.questionBlock:hover {
  background-color: #e6f7ff; /* Light blue on hover */
}

.activeQuestion {
  background-color: #cce5ff; /* Highlight active question */
  font-weight: bold; /* Emphasize selected question */
}

.selectedQuestionBlock {
  padding: 20px;
  margin-top: 20px; /* Increased space between dropdown and selected question */
  border: 1px solid #0073e6; /* Blue border to denote active selection */
  border-radius: 5px;
  background-color: #f8f9fa; /* Light background for selected question */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Added shadow for depth */
}


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

.loaderWrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 60vh;
}

.selectedQuestionBlock {
  padding: 20px;
  margin-top: 20px; /* Space between dropdown and selected question */
  border: 1px solid #0073e6; /* Blue border to denote active selection */
  border-radius: 5px;
  background-color: #f8f9fa; /* Light background for selected question */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Added shadow for depth */
}
