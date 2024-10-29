import React from 'react';
import styles from './FaqDropdown.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';
import QuestionBlock from './QuestionBlock';

const FaqDropdown = ({ contentData, onQuestionSelect, selectedQuestion }) => {
  const [isFaqOpen, setIsFaqOpen] = React.useState(false); // Initially closed

  const toggleFaq = () => setIsFaqOpen(!isFaqOpen);

  const handleQuestionSelect = (question, answer) => {
    onQuestionSelect(question, answer);
    setIsFaqOpen(false); // Close dropdown after selection
  };

  return (
    <div className={styles.faqDropdown}>
      <div className={styles.dropdownHeader} onClick={toggleFaq}>
        <span>Frequently Asked Questions</span>
        <FontAwesomeIcon icon={isFaqOpen ? faChevronUp : faChevronDown} />
      </div>
      
      <div className={`${styles.dropdownContent} ${isFaqOpen ? styles.show : styles.hide}`}>
        {contentData.map((item, index) => (
          <div 
            key={index} 
            onClick={() => handleQuestionSelect(item.question, item.answer)} 
            className={`${styles.questionBlock} ${selectedQuestion === item.question ? styles.activeQuestion : ''}`}
          >
            <span>{item.question}</span>
          </div>
        ))}
      </div>
    </div>
  );
};

export default FaqDropdown;


.faqDropdown {
  border: 1px solid rgba(95, 30, 193, 0.1); /* Light purple on hover */

  margin-top: 12px;
  border-radius: 8px;
  overflow: hidden; /* Ensures children don't overflow the parent */
  transition: box-shadow 0.3s ease; /* Smooth transition for shadow */
}

.faqDropdown:hover {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); /* Elevate on hover */
}

.dropdownHeader {
  display: flex;
  justify-content: space-between;
  align-items: center; /* Center align items vertically */
  padding: 15px 20px;
  background: linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%); /* Gradient background */
  color: #fff; /* White text color */
  cursor: pointer;
  font-weight: bold;
  font-size: 1rem;
  border-radius: 8px 8px 0 0; /* Rounded corners on top */
  transition: background-color 0.3s ease; /* Smooth background color transition */
}

.dropdownHeader:hover {
  background: linear-gradient(90deg, rgb(75, 20, 173) 0%, rgb(5, 75, 200) 100%); /* Darken gradient on hover */
}

.dropdownContent {
  padding: 10px 20px;
  border-top: 1px solid #ddd; /* Separate header from content */
  background-color: #fff; /* White background for content */
  max-height: calc(5 * 50px); /* Approximate height for 5 questions (adjust 50px as needed) */
  overflow-y: auto; /* Allow vertical scrolling */
  transition: max-height 0.5s ease, opacity 0.5s ease; /* Smooth max-height transition */
  opacity: 0; /* Initially hidden */
}

.show {
  max-height: calc(5 * 50px); /* Same as above */
  opacity: 1; /* Fully visible */
  transition: max-height 0.5s ease, opacity 0.5s ease; /* Smooth transition */
}

.hide {
  max-height: 0; /* Collapsed height */
  opacity: 0; /* Hidden */
}

.questionBlock {
  padding: 10px;
  cursor: pointer;
  font-size: .8rem;
  transition: background-color 0.3s ease, transform 0.3s ease; /* Smooth transitions for background and transform */
  border-radius: 4px; /* Rounded corners */
}

.questionBlock:hover {
  background-color: rgba(95, 30, 193, 0.1); /* Light purple on hover */
  transform: translateY(-2px); /* Slight lift effect */
}

.activeQuestion {
  background-color: rgba(15, 95, 220, 0.2); /* Highlight active question */
  font-weight: bold; /* Emphasize selected question */
}

.selectedQuestionBlock {
  padding: 20px;
  margin-top: 20px; /* Increased space between dropdown and selected question */
  border: 1px solid rgb(95, 30, 193); /* Use main theme color */
  border-radius: 5px;
  background-color: #f8f9fa; /* Light background for selected question */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Added shadow for depth */
}



/* Custom Scrollbar Styles */
.dropdownContent::-webkit-scrollbar {
  width: 8px; /* Width of the scrollbar */
}

.dropdownContent::-webkit-scrollbar-track {
  background: #f1f1f1; /* Background of the scrollbar track */
  border-radius: 8px; /* Rounded corners */
}

.dropdownContent::-webkit-scrollbar-thumb {
  background: rgb(95, 30, 193); /* Color of the scrollbar thumb */
  border-radius: 8px; /* Rounded corners */
}

.dropdownContent::-webkit-scrollbar-thumb:hover {
  background: rgb(75, 20, 173); /* Darker shade on hover */
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

      <div className={`${styles.responseSection} ${styles.leftSection}`}>
        <p><strong>Textual Response:</strong></p>
        {renderResponsePoints(answerData.textualResponse, showFullTextual)}
        <button className={styles.seeMoreButton} onClick={() => setShowFullTextual(!showFullTextual)}>
          {showFullTextual ? 'See Less' : 'See More'}
        </button>
      </div>

      <div className={`${styles.responseSection} ${styles.rightSection}`}>
        <p><strong>Citizen Experience:</strong></p>
        {renderResponsePoints([answerData.citizenExperience], showFullCitizen)}
        <button className={styles.seeMoreButton} onClick={() => setShowFullCitizen(!showFullCitizen)}>
          {showFullCitizen ? 'See Less' : 'See More'}
        </button>
      </div>

      <div className={`${styles.responseSection} ${styles.bottomLeftSection}`}>
        <p><strong>Factual Info:</strong></p>
        {renderResponsePoints([answerData.factualInfo], showFullFactual)}
        <button className={styles.seeMoreButton} onClick={() => setShowFullFactual(!showFullFactual)}>
          {showFullFactual ? 'See Less' : 'See More'}
        </button>
      </div>

      <div className={`${styles.responseSection} ${styles.bottomRightSection}`}>
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

.questionBlock {
  display: grid;
  grid-template-areas:
    "question question"
    "leftSection rightSection"
    "bottomLeftSection bottomRightSection";
  grid-gap: 20px;
  padding: 15px;
  margin: 20px 0;
  border-radius: 8px;
  background-color: #ffffff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.question {
  grid-area: question;
  font-size: 1.2rem;
  font-weight: bold;
  color: #2a2a2a;
  margin-bottom: 15px;
}

.responseSection {
  padding: 15px;
  font-size:1rem;
  background-color: #f9f9f9;
  border-left: 4px solid #0073e6;
  border-radius: 6px;
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
  font-size: .8rem;
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
