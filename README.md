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



import React, { useState } from 'react';
import FaqDropdown from './FaqDropdown';
import QuestionBlock from './QuestionBlock'; // Importing QuestionBlock if needed

const FaqSection = ({ contentData }) => {
  const [selectedQuestion, setSelectedQuestion] = useState(null);
  const [selectedAnswer, setSelectedAnswer] = useState(null);

  const onQuestionSelect = (question, answer) => {
    setSelectedQuestion(question);
    setSelectedAnswer(answer);
  };

  return (
    <div>
      <FaqDropdown 
        contentData={contentData}
        onQuestionSelect={onQuestionSelect}
        selectedQuestion={selectedQuestion}
        selectedAnswer={selectedAnswer}
      />

      {selectedQuestion && (
        <div className={styles.selectedQuestionBlock}>
          <QuestionBlock
            question={selectedQuestion}
            answerData={selectedAnswer}
          />
        </div>
      )}
    </div>
  );
};

export default FaqSection;



.selectedQuestionBlock {
  padding: 20px;
  margin-top: 20px; /* Increased space between dropdown and selected question */
  border: 2px solid #0073e6; /* Blue border to denote active selection */
  border-radius: 10px; /* Rounded corners */
  background-color: #f8f9fa; /* Light background for selected question */
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1); /* Shadow for depth */
}
