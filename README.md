import React, { useState } from 'react';
import styles from './QuestionBlock.module.css';

const QuestionBlock = ({ question, answerData }) => {
  // State for toggling visibility of each section
  const [showFullTextual, setShowFullTextual] = useState(false);
  const [showFullCitizen, setShowFullCitizen] = useState(false);
  const [showFullFactual, setShowFullFactual] = useState(false);
  const [showFullContextual, setShowFullContextual] = useState(false);

  return (
    <div className={styles.questionBlock}>
      <div className={styles.question}>{question}</div>

      {/* Textual Response Section */}
      <div className={styles.responseSection}>
        <p><strong>Textual Response:</strong> {showFullTextual ? answerData.textualResponse.join(', ') : `${answerData.textualResponse.join(', ').slice(0, 50)}...`}</p>
        <button className={styles.seeMoreButton} onClick={() => setShowFullTextual(!showFullTextual)}>
          {showFullTextual ? 'See Less' : 'See More'}
        </button>
      </div>

      {/* Citizen Experience Section */}
      <div className={styles.responseSection}>
        <p><strong>Citizen Experience:</strong> {showFullCitizen ? answerData.citizenExperience : `${answerData.citizenExperience.slice(0, 50)}...`}</p>
        <button className={styles.seeMoreButton} onClick={() => setShowFullCitizen(!showFullCitizen)}>
          {showFullCitizen ? 'See Less' : 'See More'}
        </button>
      </div>

      {/* Factual Info Section */}
      <div className={styles.responseSection}>
        <p><strong>Factual Info:</strong> {showFullFactual ? answerData.factualInfo : `${answerData.factualInfo.slice(0, 50)}...`}</p>
        <button className={styles.seeMoreButton} onClick={() => setShowFullFactual(!showFullFactual)}>
          {showFullFactual ? 'See Less' : 'See More'}
        </button>
      </div>

      {/* Contextual Section */}
      <div className={styles.responseSection}>
        <p><strong>Contextual:</strong> {showFullContextual ? answerData.contextual : `${answerData.contextual.slice(0, 50)}...`}</p>
        <button className={styles.seeMoreButton} onClick={() => setShowFullContextual(!showFullContextual)}>
          {showFullContextual ? 'See Less' : 'See More'}
        </button>
      </div>
    </div>
  );
};

export default QuestionBlock;



.questionBlock {
  padding: 15px;
  margin: 20px 0;
  border-radius: 8px;
  background-color: #ffffff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s;
}

.question {
  font-size: 1.2rem;
  font-weight: bold;
  color: #2a2a2a;
  margin-bottom: 15px;
}

.responseSection {
  margin-top: 15px;
  padding: 15px;
  background-color: #f9f9f9;
  border-left: 4px solid #0073e6;
  border-radius: 6px;
}

.seeMoreButton {
  display: inline-block;
  margin-top: 10px;
  background-color: #0073e6;
  color: white;
  padding: 5px 10px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.8rem;
  transition: background-color 0.2s;
}

.seeMoreButton:hover {
  background-color: #005bb5;
}
