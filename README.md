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
  border-radius: 10px; /* More rounded corners for a modern look */
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1); /* Soft shadow for depth */
  background-color: #ffffff; /* White background for the whole dropdown */
}

.dropdownHeader {
  display: flex;
  justify-content: space-between;
  align-items: center; /* Center align items vertically */
  padding: 15px;
  background-color: #0073e6; /* Blue header background */
  color: #ffffff; /* White text color for contrast */
  font-weight: bold;
  border-radius: 10px 10px 0 0; /* Rounded top corners */
  cursor: pointer;
}

.dropdownContent {
  padding: 10px 15px;
  border-top: 1px solid #ddd; /* Separator line between header and content */
  border-radius: 0 0 10px 10px; /* Rounded bottom corners */
}

.questionBlock {
  padding: 12px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  border-radius: 5px; /* Slightly rounded corners */
  margin-bottom: 8px; /* Space between items */
  background-color: #f9f9f9; /* Light grey background for question items */
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05); /* Light shadow */
}

.questionBlock:hover {
  background-color: #e6f7ff; /* Light blue on hover */
}

.activeQuestion {
  background-color: #cce5ff; /* Highlight active question */
  font-weight: bold; /* Emphasize selected question */
  border: 2px solid #0073e6; /* Border to indicate active state */
}

.selectedQuestionBlock {
  padding: 20px;
  margin-top: 20px; /* Increased space between dropdown and selected question */
  border: 2px solid #0073e6; /* Blue border to denote active selection */
  border-radius: 10px; /* Rounded corners */
  background-color: #f8f9fa; /* Light background for selected question */
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1); /* Shadow for depth */
}

