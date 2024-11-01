import React, { useState } from 'react';
import styles from './QuestionBlock.module.css';

const QuestionBlock = ({ question, answerData }) => {
  const [showFullTextual, setShowFullTextual] = useState(false);
  const [showFullCitizen, setShowFullCitizen] = useState(false);
  const [showFullFactual, setShowFullFactual] = useState(false);
  const [showFullContextual, setShowFullContextual] = useState(false);

  const renderResponsePoints = (points, showFull) => (
    <div>
      {points.slice(0, showFull ? points.length : 3).map((point, index) => (
        <p key={index}>{point}</p>
      ))}
      {points.length > 3 && (
        <button onClick={() => showFull((prev) => !prev)}>
          {showFull ? 'Show Less' : 'Show More'}
        </button>
      )}
    </div>
  );

  const renderImage = (url, altText) => (
    url ? <img src={url} alt={altText} className={styles.image} /> : <p>Image not available</p>
  );

  return (
    <div className={styles.questionBlock}>
      <div className={styles.question}>{question}</div>

      {/* Textual Response */}
      <div className={`${styles.responseSection} ${showFullTextual ? styles.expanded : ''}`}>
        <p><strong>Textual Response:</strong></p>
        {renderResponsePoints(answerData.textualResponse, showFullTextual)}
      </div>

      {/* Citizen Experience */}
      <div className={`${styles.responseSection} ${showFullCitizen ? styles.expanded : ''}`}>
        <p><strong>Citizen Experience:</strong></p>
        {renderImage(answerData.citizenExperience, "Citizen Experience Image")}
      </div>

      {/* Factual Info */}
      <div className={`${styles.responseSection} ${showFullFactual ? styles.expanded : ''}`}>
        <p><strong>Factual Info:</strong></p>
        {renderImage(answerData.factualInfo, "Factual Info Image")}
      </div>

      {/* Contextual */}
      <div className={`${styles.responseSection} ${showFullContextual ? styles.expanded : ''}`}>
        <p><strong>Contextual:</strong></p>
        {renderResponsePoints([answerData.contextual], showFullContextual)}
      </div>
    </div>
  );
};

export default QuestionBlock;
