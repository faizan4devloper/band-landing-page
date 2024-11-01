import React, { useState, useRef, useEffect } from 'react';
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
  const [expandedSections, setExpandedSections] = useState({});
  const sectionRefs = {
    textualResponse: useRef(null),
    citizenExperience: useRef(null),
    factualInfo: useRef(null),
    contextual: useRef(null)
  };

  // Toggle function to expand or collapse a section
  const toggleSection = (section) => {
    setExpandedSections((prev) => ({
      ...prev,
      [section]: !prev[section]
    }));
  };

  // Function to dynamically set max height based on content length
  const getSectionHeight = (section) => {
    return expandedSections[section] ? `${sectionRefs[section].current.scrollHeight}px` : '160px';
  };

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

  return (
    <div className={styles.questionBlock}>
      <div className={styles.question}>{question}</div>

      {/* Textual Response */}
      <div
        ref={sectionRefs.textualResponse}
        className={`${styles.responseSection} ${expandedSections.textualResponse ? styles.expanded : ''}`}
        style={{ maxHeight: getSectionHeight('textualResponse') }}
      >
        <p><strong>Textual Response:</strong></p>
        {renderResponsePoints(answerData.textualResponse)}
        {answerData.textualImage && <img src={answerData.textualImage} alt="Textual Response" className={styles.responseImage} />}
        {answerData.textualResponse && answerData.textualResponse.length > 2 && (
          <button className={styles.seeMoreButton} onClick={() => toggleSection('textualResponse')}>
            {expandedSections.textualResponse ? 'See Less' : 'See More'}
          </button>
        )}
      </div>

      {/* Other Sections (Citizen Experience, Factual Info, Contextual) */}
      {/* Repeat the structure for each section like above */}
    </div>
  );
};

export default QuestionBlock;


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
  padding: 15px;
  background-color: #ffffff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.question {
  grid-area: question;
  font-size: 1.3rem;
  font-weight: bold;
  color: #2a2a2a;
  margin-bottom: 15px;
}

.responseSection {
  padding: 15px;
  font-size: 1rem;
  background-color: #f9f9f9;
  border-left: 4px solid #0073e6;
  border-radius: 6px;
  position: relative;
  overflow: hidden;
  transition: max-height 0.4s ease, padding 0.4s ease;
  width: 100%;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.responseSection.expanded {
  overflow-y: auto;
}

.responseList {
  margin: 10px 0;
  padding-left: 20px;
}

.responseItem {
  margin-bottom: 8px;
  font-size: 0.9rem;
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

.seeMoreButton {
  display: inline-block;
  margin-top: 10px;
  background: linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%);
  color: white;
  padding: 6px 12px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.9rem;
  transition: background-color 0.2s;
}

.seeMoreButton:hover {
  background-color: #005bb5;
}

.responseSection.expanded::-webkit-scrollbar {
  width: 6px;
}

.responseSection.expanded::-webkit-scrollbar-thumb {
  background-color: rgba(15, 95, 220, 1);
  border-radius: 10px;
}
