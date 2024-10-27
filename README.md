import React from 'react';
import styles from './QuestionBlock.module.css';

const QuestionBlock = ({ question, answerData }) => {
  return (
    <div className={styles.questionBlock}>
      <div className={styles.card}>
        <h3 className={styles.question}>{question}</h3>
        <div className={styles.dataGrid}>
          <div className={styles.cardItem}>
            <h4>Textual Response</h4>
            <p>{answerData.textualResponse.join(', ')}</p>
          </div>
          <div className={styles.cardItem}>
            <h4>Citizen Experience</h4>
            <p>{answerData.citizenExperience}</p>
          </div>
          <div className={styles.cardItem}>
            <h4>Factual Info</h4>
            <p>{answerData.factualInfo}</p>
          </div>
          <div className={styles.cardItem}>
            <h4>Contextual</h4>
            <p>{answerData.contextual}</p>
          </div>
        </div>
      </div>
    </div>
  );
};

export default QuestionBlock;



.questionBlock {
  padding: 15px; /* Comfortable padding */
  margin: 15px 0; /* Top and bottom margin for spacing */
  border-radius: 8px; /* Rounded corners */
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1); /* Soft shadow for depth */
  transition: transform 0.2s; /* Smooth scale effect on hover */
  background-color: #ffffff; /* White background for clarity */
}

.card {
  padding: 20px; /* Padding for the card */
  border-radius: 8px; /* Rounded corners */
  background-color: #fefefe; /* Slightly different background */
  border: 1px solid #e1e1e1; /* Light border for definition */
  transition: box-shadow 0.2s; /* Smooth shadow transition */
}

.card:hover {
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2); /* Deeper shadow on hover */
  transform: translateY(-2px); /* Slight upward movement on hover */
}

.question {
  font-size: 1.5rem; /* Larger font for question text */
  color: #333; /* Dark text color */
  margin-bottom: 15px; /* Space below question */
}

.dataGrid {
  display: grid; /* Grid layout for data display */
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); /* Responsive grid */
  gap: 15px; /* Space between grid items */
}

.cardItem {
  padding: 10px; /* Padding for each item */
  background-color: #f9f9f9; /* Light background for item */
  border-radius: 5px; /* Rounded corners for items */
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1); /* Light shadow for items */
  transition: transform 0.2s; /* Smooth scale effect */
}

.cardItem:hover {
  transform: scale(1.02); /* Slight scaling effect on hover */
}

.cardItem h4 {
  font-size: 1.2rem; /* Font size for item headings */
  color: #0073e6; /* Color for headings */
  margin-bottom: 5px; /* Space below heading */
}

.cardItem p {
  color: #444; /* Dark grey for item text */
  font-size: 1rem; /* Normal font size for text */
  margin: 0; /* Remove default margin */
}







import React, { useState } from 'react';
import styles from './QuestionBlock.module.css';

const QuestionBlock = ({ question, answerData }) => {
  const [isExpanded, setIsExpanded] = useState(false);

  const toggleExpand = () => {
    setIsExpanded(!isExpanded);
  };

  return (
    <div className={styles.card} onClick={toggleExpand}>
      <div className={styles.cardHeader}>
        <h3>{question}</h3>
        <span className={styles.arrow}>{isExpanded ? '▼' : '►'}</span>
      </div>
      {isExpanded && (
        <div className={styles.cardBody}>
          <div className={styles.dataPoint}>
            <h4>Textual Response</h4>
            <p>{answerData.textualResponse.join(', ')}</p>
          </div>
          <div className={styles.dataPoint}>
            <h4>Citizen Experience</h4>
            <p>{answerData.citizenExperience}</p>
          </div>
          <div className={styles.dataPoint}>
            <h4>Factual Info</h4>
            <p>{answerData.factualInfo}</p>
          </div>
          <div className={styles.dataPoint}>
            <h4>Contextual</h4>
            <p>{answerData.contextual}</p>
          </div>
        </div>
      )}
    </div>
  );
};

export default QuestionBlock;



.card {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 15px;
  margin: 15px;
  cursor: pointer;
  transition: box-shadow 0.2s ease;
}

.card:hover {
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
}

.cardHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.cardBody {
  margin-top: 10px;
}

.dataPoint {
  margin-bottom: 10px;
}

.dataPoint h4 {
  margin-bottom: 5px;
  font-size: 1.1rem;
  color: #0073e6;
}
