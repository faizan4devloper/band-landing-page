import React, { useState } from 'react';
import styles from './QuestionBlock.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPlus, faMinus } from '@fortawesome/free-solid-svg-icons';

const QuestionBlock = ({ question, answerData }) => {
  const [isExpanded, setIsExpanded] = useState(false); // State to manage collapsible sections

  const toggleExpand = () => {
    setIsExpanded(prev => !prev); // Toggle the expansion state
  };

  return (
    <div className={styles.questionBlock}>
      <div className={styles.question} onClick={toggleExpand}>
        <span>{question}</span>
        <FontAwesomeIcon icon={isExpanded ? faMinus : faPlus} className={styles.toggleIcon} />
      </div>

      {isExpanded && (
        <div className={styles.answer}>
          <div className={styles.section}>
            <h3>Textual Response</h3>
            <p>{answerData.textualResponse.join(', ')}</p>
          </div>
          
          <div className={styles.section}>
            <h3>Citizen Experience</h3>
            <p>{answerData.citizenExperience}</p>
          </div>
          
          <div className={styles.section}>
            <h3>Factual Info</h3>
            <p>{answerData.factualInfo}</p>
          </div>
          
          <div className={styles.section}>
            <h3>Contextual</h3>
            <p>{answerData.contextual}</p>
          </div>
        </div>
      )}
    </div>
  );
};

export default QuestionBlock;


.questionBlock {
  padding: 15px; /* Comfortable padding */
  margin: 15px 0; /* Top and bottom margin for spacing */
  border-radius: 8px; /* Rounded corners */
  background-color: #ffffff; /* White background */
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1); /* Light shadow for depth */
  transition: box-shadow 0.3s; /* Smooth transition for hover */
}

.question {
  display: flex; /* Flexbox for aligning items */
  justify-content: space-between; /* Space between question text and icon */
  align-items: center; /* Center alignment of items */
  cursor: pointer; /* Pointer cursor for clickable question */
  font-size: 1.3rem; /* Font size for question text */
  font-weight: bold; /* Bold for emphasis */
  color: #2a2a2a; /* Dark text color */
  margin-bottom: 10px; /* Space below question text */
}

.toggleIcon {
  margin-left: 10px; /* Space between question text and icon */
  color: #0073e6; /* Color for the toggle icon */
}

.answer {
  margin-top: 10px; /* Space above answer text */
}

.section {
  margin-bottom: 15px; /* Space between sections */
  padding: 10px; /* Padding around each section */
  background-color: #f9f9f9; /* Light background for sections */
  border-left: 4px solid #0073e6; /* Blue left border for visual interest */
  border-radius: 4px; /* Slight rounding for section */
}

.section h3 {
  margin-bottom: 5px; /* Space below section title */
  font-size: 1.2rem; /* Font size for section titles */
  color: #2a2a2a; /* Title color */
}

.section p {
  color: #444; /* Dark grey for answers */
  font-size: 1rem; /* Normal font size for answers */
}
