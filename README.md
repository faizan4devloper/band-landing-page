import React, { useState } from 'react';
import styles from './QuestionBlock.module.css';

// Utility function to convert URLs in text to anchor tags
const parseLinks = (text) => {
  const urlPattern = /(https?:\/\/[^\s]+)/g;
  return text.split(urlPattern).map((part, index) =>
    urlPattern.test(part) ? (
      <a key={index} href={part} target="_blank" rel="noopener noreferrer" className={styles.link}>
        {part}
      </a>
    ) : (
      part
    )
  );
};

const QuestionBlock = ({ question, answerData }) => {
  const [showFullTextual, setShowFullTextual] = useState(false);
  const [showFullCitizen, setShowFullCitizen] = useState(false);
  const [showFullFactual, setShowFullFactual] = useState(false);
  const [showFullContextual, setShowFullContextual] = useState(false);

  const renderResponsePoints = (responseArray, isExpanded) => (
    <ul className={styles.responseList}>
      {(isExpanded ? responseArray : responseArray.slice(0, 2)).map((item, index) => (
        <li key={index} className={styles.responseItem}>
          {parseLinks(item)}
        </li>
      ))}
    </ul>
  );

  return (
    <div className={styles.questionBlock}>
      <div className={styles.question}>{question}</div>

      <div className={`${styles.responseSection} ${showFullTextual ? styles.expanded : ''}`}>
        <p><strong>Textual Response:</strong></p>
        {renderResponsePoints(answerData.textualResponse, showFullTextual)}
        <button className={styles.seeMoreButton} onClick={() => setShowFullTextual(!showFullTextual)}>
          {showFullTextual ? 'See Less' : 'See More'}
        </button>
      </div>

      <div className={`${styles.responseSection} ${showFullCitizen ? styles.expanded : ''}`}>
        <p><strong>Citizen Experience:</strong></p>
        {renderResponsePoints([answerData.citizenExperience], showFullCitizen)}
        <button className={styles.seeMoreButton} onClick={() => setShowFullCitizen(!showFullCitizen)}>
          {showFullCitizen ? 'See Less' : 'See More'}
        </button>
      </div>

      <div className={`${styles.responseSection} ${showFullFactual ? styles.expanded : ''}`}>
        <p><strong>Factual Info:</strong></p>
        {renderResponsePoints([answerData.factualInfo], showFullFactual)}
        <button className={styles.seeMoreButton} onClick={() => setShowFullFactual(!showFullFactual)}>
          {showFullFactual ? 'See Less' : 'See More'}
        </button>
      </div>

      <div className={`${styles.responseSection} ${showFullContextual ? styles.expanded : ''}`}>
        <p><strong>Contextual:</strong></p>
        {renderResponsePoints([answerData.contextual], showFullContextual)}
        <button className={styles.seeMoreButton} onClick={() => setShowFullContextual(!showFullContextual)}>
          {showFullContextual ? 'See Less' : 'See More'}
        </button>
      </div>
    </div>
  );
};

export default QuestionBlock;




.responseSection {
  padding: 15px;
  font-size: 1rem;
  background-color: #f9f9f9;
  border-left: 4px solid #0073e6;
  border-radius: 6px;
  max-height: 80px; /* Collapsed height */
  overflow: hidden;
  transition: max-height 0.4s ease; /* Smooth transition */
}

.responseSection.expanded {
  max-height: 300px; /* Expanded height */
}
