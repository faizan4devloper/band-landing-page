import React from 'react';
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
  const renderResponsePoints = (responseArray) => {
    const arrayToRender = Array.isArray(responseArray) ? responseArray : [responseArray];
    return (
      <ul className={styles.responseList}>
        {arrayToRender.map((item, index) => (
          <li key={index} className={styles.responseItem}>
            {parseLinks(item)}
          </li>
        ))}
      </ul>
    );
  };

  const renderImage = (imageUrl, altText) =>
    imageUrl ? (
      <img src={imageUrl} alt={altText} className={styles.responseImage} />
    ) : null;

  return (
    <div className={styles.questionBlock}>
      <div className={styles.question}>{question}</div>

      {/* Textual Response */}
      <div className={styles.responseSection}>
        <p><strong>Textual Response:</strong></p>
        {renderResponsePoints(answerData.textualResponse)}
        {renderImage(answerData.textualImage, "Textual Response Image")}
      </div>

      {/* Citizen Experience */}
      <div className={styles.responseSection}>
        <p><strong>Citizen Experience:</strong></p>
        {renderImage(answerData.citizenReview, "Citizen Experience Image")}
      </div>

      {/* Factual Info */}
      <div className={styles.responseSection}>
        <p><strong>Factual Info:</strong></p>
        {renderImage(answerData.factualData, "Factual Info Image")}
      </div>

      {/* Contextual */}
      <div className={styles.responseSection}>
        <p><strong>Contextual:</strong></p>
        {renderResponsePoints([answerData.contextual])}
        {renderImage(answerData.contextualImage, "Contextual Info Image")}
      </div>
    </div>
  );
};

export default QuestionBlock;


.responseSection {
  padding: 15px;
  font-size: 0.8rem;
  background-color: #f9f9f9;
  border-left: 4px solid #0073e6;
  border-radius: 6px;
  position: relative;
  max-height: 150px; /* Set a maximum height for scrollable content */
  overflow-y: auto; /* Enable vertical scrolling */
  width: 100%;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: max-height 0.4s ease, padding 0.4s ease;
}

/* Custom scrollbar styling */
.responseSection::-webkit-scrollbar {
  width: 6px;
}

.responseSection::-webkit-scrollbar-thumb {
  background-color: rgba(15, 95, 220, 1);
  border-radius: 10px;
}

/* Remove the expanded class and related styles if no longer needed */
