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
    imageUrl ? <img src={imageUrl} alt={altText} className={styles.responseImage} /> : null
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
        {renderImage(answerData.citizenImage, "Citizen Experience Image")}
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
        {renderImage(answerData.factualImage, "Factual Info Image")}
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
