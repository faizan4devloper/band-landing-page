import React, { useState } from 'react';
import styles from './QuestionBlock.module.css';

const QuestionBlock = ({ question, answerData }) => {
  const [expanded, setExpanded] = useState(false);

  const toggleAccordion = () => {
    setExpanded(!expanded);
  };

  return (
    <div className={styles.questionBlock}>
      <div className={styles.question} onClick={toggleAccordion}>
        <h3>{question}</h3>
        <span className={styles.toggleIcon}>{expanded ? '-' : '+'}</span>
      </div>
      {expanded && (
        <div className={styles.answerContainer}>
          <div className={styles.answer}>
            <p><strong>Textual Response:</strong> {answerData.textualResponse.join(', ')}</p>
            <p><strong>Citizen Experience:</strong> {answerData.citizenExperience}</p>
            <p><strong>Factual Info:</strong> {answerData.factualInfo}</p>
            <p><strong>Contextual:</strong> {answerData.contextual}</p>
          </div>
        </div>
      )}
    </div>
  );
};

export default QuestionBlock;



.questionBlock {
  padding: 15px; /* Comfortable padding */
  margin: 10px 0; /* Spacing between question blocks */
  border-radius: 5px; /* Rounded corners */
  background-color: #ffffff; /* White background */
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1); /* Subtle shadow */
  transition: box-shadow 0.3s; /* Transition effect for shadow */
}

.question {
  font-size: 1.5rem; /* Larger font for question */
  color: #333; /* Dark text color */
  cursor: pointer; /* Pointer cursor on hover */
  display: flex; /* Flex layout for alignment */
  justify-content: space-between; /* Space between question and icon */
  align-items: center; /* Center alignment */
}

.answerContainer {
  padding: 10px 0; /* Padding for answer container */
  margin-top: 10px; /* Space above answer text */
  border-top: 1px solid #e1e1e1; /* Top border for separation */
}

.answer {
  font-size: 1rem; /* Normal font size for answers */
  color: #444; /* Dark grey text color */
}

.toggleIcon {
  font-size: 1.5rem; /* Larger icon for visibility */
  color: #0073e6; /* Color for toggle icon */
}
