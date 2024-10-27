import React from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';
import styles from './QuestionBlock.module.css';
import GridAnswer from './GridAnswer';

const QuestionBlock = ({ question, answerData, isOpen, toggle }) => (
  <div className={styles.questionBlock}>
    <div className={styles.question} onClick={toggle}>
      {question}
      <FontAwesomeIcon icon={isOpen ? faChevronUp : faChevronDown} className={styles.chevronIcon} />
    </div>
    {isOpen && <GridAnswer answerData={answerData} />}
  </div>
);

export default QuestionBlock;




.questionBlock {
  margin-bottom: 15px;
  border: 1px solid #ddd;
  border-radius: 10px;
  background-color: #ffffff;
  transition: background-color 0.3s ease, box-shadow 0.3s ease;
  overflow: hidden;
}

.questionBlock:hover {
  background-color: #f0f0f0;
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.1);
}

.question {
  font-size: 1rem;
  font-weight: bold;
  padding: 14px;
  cursor: pointer;
  color: #2a2a2a;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: color 0.3s ease;
}

.question:hover {
  color: #0073e6;
}

.chevronIcon {
  font-size: 1rem;
  color: #888;
  margin-left: 10px;
  transition: transform 0.3s ease, color 0.3s ease;
}
