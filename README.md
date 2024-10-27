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

export default FaqDropdown;



.faqDropdown {
  border: 1px solid #ddd;
  margin-top: 20px;
  border-radius: 5px;
}

.dropdownHeader {
  display: flex;
  justify-content: space-between;
  align-items: center; /* Center align items vertically */
  padding: 15px;
  background-color: #f5f5f5;
  cursor: pointer;
  font-weight: bold;
  border-bottom: 1px solid #ddd;
}

.dropdownContent {
  padding: 10px 15px;
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
  margin-top: 15px; /* Space between dropdown and selected question */
  border: 1px solid #0073e6; /* Blue border to denote active selection */
  border-radius: 5px;
  background-color: #f8f9fa; /* Light background for selected question */
}
