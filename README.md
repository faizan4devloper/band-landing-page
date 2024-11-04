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

  const renderResponsePoints = (responseArray, isExpanded) => {
    const arrayToRender = Array.isArray(responseArray) ? responseArray : [responseArray];
    return (
      <ul className={styles.responseList}>
        {(isExpanded ? arrayToRender : arrayToRender.slice(0, 2)).map((item, index) => (
          <li key={index} className={styles.responseItem}>
            {parseLinks(item)}
          </li>
        ))}
      </ul>
    );
  };

  const renderImage = (imageUrl, altText) => (
    imageUrl ? (
      <img src={imageUrl}
        alt={altText}
        className={styles.responseImage}
      />
    ) : null
  );

  return (
    <div className={styles.questionBlock}>
      <div className={styles.question}>{question}</div>

      {/* Textual Response */}
      <div className={`${styles.responseSection} ${showFullTextual ? styles.expanded : ''}`}>
        <p><strong>Textual Response:</strong></p>
        {renderResponsePoints(answerData.textualResponse, showFullTextual)}
        {renderImage(answerData.textualImage, "Textual Response Image")}
        {answerData.textualResponse && answerData.textualResponse.length > 2 && (
          <button className={styles.seeMoreButton} onClick={() => setShowFullTextual(!showFullTextual)}>
            {showFullTextual ? 'See Less' : 'See More'}
          </button>
        )}
      </div>

      {/* Citizen Experience */}
      <div className={`${styles.responseSection} ${showFullCitizen ? styles.expanded : ''}`}>
        <p><strong>Citizen Experience:</strong></p>
        {renderResponsePoints([answerData.citizenExperience], showFullCitizen)}
        {renderImage(answerData.citizenReview, "Citizen Experience Image")}
        {answerData.citizenExperience && answerData.citizenExperience.length > 50 && (
          <button className={styles.seeMoreButton} onClick={() => setShowFullCitizen(!showFullCitizen)}>
            {showFullCitizen ? 'See Less' : 'See More'}
          </button>
        )}
      </div>

      {/* Factual Info */}
      <div className={`${styles.responseSection} ${showFullFactual ? styles.expanded : ''}`}>
        <p><strong>Factual Info:</strong></p>
        {renderResponsePoints([answerData.factualInfo], showFullFactual)}
        {renderImage(answerData.factualData, "Factual Info Image")}
        {answerData.factualInfo && answerData.factualInfo.length > 50 && (
          <button className={styles.seeMoreButton} onClick={() => setShowFullFactual(!showFullFactual)}>
            {showFullFactual ? 'See Less' : 'See More'}
          </button>
        )}
      </div>

      {/* Contextual */}
      <div className={`${styles.responseSection} ${showFullContextual ? styles.expanded : ''}`}>
        <p><strong>Contextual:</strong></p>
        {renderResponsePoints([answerData.contextual], showFullContextual)}
        {renderImage(answerData.contextualImage, "Contextual Info Image")}
        {answerData.contextual && answerData.contextual.length > 50 && (
          <button className={styles.seeMoreButton} onClick={() => setShowFullContextual(!showFullContextual)}>
            {showFullContextual ? 'See Less' : 'See More'}
          </button>
        )}
      </div>
    </div>
  );
};

export default QuestionBlock;




.questionBlock {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: auto;
  gap: 20px;
  position: relative;
  margin: 20px 0;
  padding: 15px;
  border-radius: 8px;
  background-color: #ffffff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.question {
  grid-column: span 2;
  font-size: 1.3rem;
  font-weight: bold;
  color: #2a2a2a;
  margin-bottom: 15px;
}

.responseSection {
  padding: 15px;
  font-size: 0.8rem;
  background-color: #f9f9f9;
  border-left: 4px solid #0073e6;
  border-radius: 6px;
  overflow: hidden;
  max-height: 150px; /* Set fixed height to avoid layout shift */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  transition: max-height 0.4s ease;
}

.responseSection.expanded {
  max-height: 100%; /* Allow full expansion */
}

.responseList {
  margin: 10px 0;
  padding-left: 20px;
  overflow-y: auto;
  max-height: 80px; /* Limit list height */
}

.responseItem {
  margin-bottom: 8px;
  font-size: 0.8rem;
  line-height: 1.6;
}

.link {
  color: #0073e6;
  text-decoration: underline;
  transition: color 0.2s;
}

.link:hover {
  color: #005bb5;
}

.responseImage {
  max-width: 100%;
  height: auto;
  margin-top: 10px;
}

.seeMoreButton {
  display: inline-block;
  margin-top: 10px;
  background: linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%);
  color: white;
  padding: 6px 12px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.8rem;
  transition: background-color 0.2s;
}

.seeMoreButton:hover {
  background-color: #005bb5;
}

.responseSection.expanded::-webkit-scrollbar {
  width: 6px;
}

.responseSection.expanded::-webkit-scroll
