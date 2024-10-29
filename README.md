.responseSection {
  padding: 15px;
  font-size: 1rem;
  background-color: #f9f9f9;
  border-left: 4px solid #0073e6;
  border-radius: 6px;
  position: relative;
  overflow: hidden;
  transition: max-height 0.4s ease, opacity 0.4s ease;
  max-height: 80px; /* Collapsed height */
  opacity: 1;
  width: 100%; /* Set width to 100% to fill parent container */
  max-width: 400px; /* Set a maximum width */
}

.responseSection.expanded {
  max-height: 300px; /* Expanded height */
  overflow-y: auto; /* Enable vertical scrolling */
  z-index: 1;
}

/* Optional: Customize scrollbar appearance */
.responseSection.expanded::-webkit-scrollbar {
  width: 8px;
}

.responseSection.expanded::-webkit-scrollbar-thumb {
  background-color: #0073e6;
  border-radius: 4px;
}

.responseSection.expanded::-webkit-scrollbar-thumb:hover {
  background-color: #005bb5;
}







.questionBlock {
  display: grid;
  grid-template-areas:
    "question question"
    "leftSection rightSection"
    "bottomLeftSection bottomRightSection";
  grid-gap: 20px;
  position: relative;
  margin: 20px 0;
  border-radius: 8px;
}

.responseSection {
  padding: 15px;
  font-size: 1rem;
  background-color: #f9f9f9;
  border-left: 4px solid #0073e6;
  border-radius: 6px;
  position: relative;
  overflow: hidden;
  transition: max-height 0.4s ease, opacity 0.4s ease;
  max-height: 80px; /* Collapsed height */
  opacity: 1;
}

.responseSection.expanded {
  max-height: 300px; /* Expanded height */
  z-index: 1;
}

.leftSection {
  grid-area: leftSection;
}

.rightSection {
  grid-area: rightSection;
}

.bottomLeftSection {
  grid-area: bottomLeftSection;
}

.bottomRightSection {
  grid-area: bottomRightSection;
}

.responseList {
  margin: 10px 0;
  padding-left: 20px;
}

.responseItem {
  margin-bottom: 8px;
  font-size: 0.8rem;
  line-height: 1.5;
}

.link {
  color: #0073e6;
  text-decoration: underline;
  transition: color 0.2s;
}

.link:hover {
  color: #005bb5;
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
