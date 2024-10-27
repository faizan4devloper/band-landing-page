import React from 'react';
import styles from './FaqDropdown.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';
import QuestionBlock from './QuestionBlock';

const FaqDropdown = ({ contentData, onQuestionSelect, selectedQuestion }) => {
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
      
      <div className={`${styles.dropdownContent} ${isFaqOpen ? styles.show : styles.hide}`}>
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
    </div>
  );
};

export default FaqDropdown;




.faqDropdown {
  border: 1px solid #ddd;
  margin-top: 20px;
  border-radius: 8px;
  overflow: hidden; /* Ensures children don't overflow the parent */
  transition: box-shadow 0.3s ease; /* Smooth transition for shadow */
}

.faqDropdown:hover {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); /* Elevate on hover */
}

.dropdownHeader {
  display: flex;
  justify-content: space-between;
  align-items: center; /* Center align items vertically */
  padding: 15px 20px;
  background-color: #0073e6; /* Primary color */
  color: white; /* Text color */
  cursor: pointer;
  font-weight: bold;
  border-radius: 8px 8px 0 0; /* Rounded corners on top */
  transition: background-color 0.3s ease; /* Smooth background color transition */
}

.dropdownHeader:hover {
  background-color: #005bb5; /* Darken on hover */
}

.dropdownContent {
  padding: 10px 20px;
  border-top: 1px solid #ddd; /* Separate header from content */
  background-color: #fff; /* White background for content */
  max-height: 0; /* Collapsed height */
  overflow: hidden; /* Prevent overflow */
  transition: max-height 0.5s ease, opacity 0.5s ease; /* Smooth max-height transition */
  opacity: 0; /* Initially hidden */
}

.show {
  max-height: 500px; /* Arbitrary value for max height */
  opacity: 1; /* Fully visible */
}

.hide {
  max-height: 0; /* Collapsed height */
  opacity: 0; /* Hidden */
}

.questionBlock {
  padding: 10px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease; /* Smooth transitions for background and transform */
  border-radius: 4px; /* Rounded corners */
}

.questionBlock:hover {
  background-color: #e6f7ff; /* Light blue on hover */
  transform: translateY(-2px); /* Slight lift effect */
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
