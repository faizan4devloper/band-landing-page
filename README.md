import React, { useState } from 'react';
import QuestionBlock from './QuestionBlock';
import styles from './FaqDropdown.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';

const FaqDropdown = ({ contentData, onQuestionSelect }) => {
  const [isFaqOpen, setIsFaqOpen] = useState(true); // Keep it open by default

  const toggleFaq = () => setIsFaqOpen(!isFaqOpen);

  const handleQuestionClick = (question) => {
    onQuestionSelect(question);
    // No need to close the dropdown here
  };

  return (
    <div className={styles.faqDropdown}>
      <div className={styles.dropdownHeader} onClick={toggleFaq}>
        <span>Frequently Asked Questions</span>
        <FontAwesomeIcon icon={isFaqOpen ? faChevronUp : faChevronDown} />
      </div>
      {isFaqOpen && (
        <div className={styles.dropdownContent}>
          {contentData.map((item, index) => (
            <div key={index} onClick={() => handleQuestionClick(item.question)}>
              <QuestionBlock
                question={item.question}
                answerData={item.answer}
              />
            </div>
          ))}
        </div>
      )}
    </div>
  );
};

export default FaqDropdown;
