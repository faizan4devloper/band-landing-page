import React from 'react';
import styles from './QuestionBlock.module.css';

const QuestionBlock = ({ question, answerData, onReset }) => (
  <div className={styles.questionBlock}>
    <div className={styles.question}>
      {question}
    </div>
    <div className={styles.answer}>
      <p><strong>Textual Response:</strong> {answerData.textualResponse.join(', ')}</p>
      <p><strong>Citizen Experience:</strong> {answerData.citizenExperience}</p>
      <p><strong>Factual Info:</strong> {answerData.factualInfo}</p>
      <p><strong>Contextual:</strong> {answerData.contextual}</p>
    </div>
    <button className={styles.resetButton} onClick={onReset}>
      Choose Different Question
    </button>
  </div>
);

export default QuestionBlock;
