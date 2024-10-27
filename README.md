.faqDropdown {
  border-radius: 8px;
  overflow: hidden;
  width: 100%;
  max-width: 800px;
  margin: 20px auto;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
  background: linear-gradient(135deg, #f5f5f5 0%, #ffffff 100%);
  transition: transform 0.3s ease-in-out;
}

.dropdownHeader {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 18px 24px;
  font-size: 1.2rem;
  font-weight: bold;
  background-color: #1f4b99;
  color: #ffffff;
  cursor: pointer;
  transition: background-color 0.3s ease, color 0.3s ease;
}

.dropdownHeader:hover {
  background-color: #17427d;
  color: #e1e1e1;
}

.dropdownContent {
  padding: 20px;
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.6s ease;
  background-color: #ffffff;
}

.dropdownContent.open {
  max-height: 1000px; /* Large enough to fit content */
}

.icon {
  transition: transform 0.3s ease;
}

.icon.rotate {
  transform: rotate(180deg);
}




import React, { useState } from 'react';
import QuestionBlock from './QuestionBlock';
import styles from './FaqDropdown.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown } from '@fortawesome/free-solid-svg-icons';

const FaqDropdown = ({ contentData }) => {
  const [isFaqOpen, setIsFaqOpen] = useState(false);
  const toggleFaq = () => setIsFaqOpen(!isFaqOpen);

  return (
    <div className={styles.faqDropdown}>
      <div className={styles.dropdownHeader} onClick={toggleFaq}>
        <h2>Frequently Asked Questions</h2>
        <FontAwesomeIcon
          icon={faChevronDown}
          className={`${styles.icon} ${isFaqOpen ? styles.rotate : ''}`}
        />
      </div>
      <div className={`${styles.dropdownContent} ${isFaqOpen ? 'open' : ''}`}>
        {contentData.map((item, index) => (
          <QuestionBlock
            key={index}
            question={item.question}
            answerData={item.answer}
          />
        ))}
      </div>
    </div>
  );
};

export default FaqDropdown;



.questionBlock {
  padding: 15px;
  margin-bottom: 10px;
  border-bottom: 1px solid #e1e1e1;
  transition: background-color 0.3s ease;
}

.questionBlock:hover {
  background-color: #f9f9ff;
  border-radius: 5px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.05);
}

.question {
  font-size: 1.1rem;
  font-weight: 600;
  color: #333;
  cursor: pointer;
}

.answer {
  margin-top: 8px;
  font-size: 0.95rem;
  color: #555;
  line-height: 1.6;
}

.answer p {
  margin: 4px 0;
}

.answer strong {
  color: #1f4b99;
}
