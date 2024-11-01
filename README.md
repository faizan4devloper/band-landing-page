import React from 'react';
import styles from './QuestionBlock.module.css';

const QuestionBlock = ({ question, answerData }) => {
  return (
    <div className={styles.questionBlock}>
      <h3>{question}</h3>
      {answerData.textualResponse && (
        <div className={styles.textualResponse}>
          {answerData.textualResponse.map((line, index) => (
            <p key={index}>{line}</p>
          ))}
        </div>
      )}
      
      {/* Display image if URL is present */}
      {answerData.factualInfo && answerData.factualInfo.startsWith('http') && (
        <div className={styles.imageContainer}>
          <img
            src={answerData.factualInfo}
            alt="Factual Information"
            className={styles.image}
            onError={(e) => { e.target.style.display = 'none'; }} // Hide if image fails to load
          />
        </div>
      )}

      {/* Optional: Display citizen experience image if available */}
      {answerData.citizenExperience && answerData.citizenExperience.startsWith('http') && (
        <div className={styles.imageContainer}>
          <img
            src={answerData.citizenExperience}
            alt="Citizen Experience"
            className={styles.image}
            onError={(e) => { e.target.style.display = 'none'; }}
          />
        </div>
      )}

      <p>{answerData.contextual}</p>
    </div>
  );
};

export default QuestionBlock;
