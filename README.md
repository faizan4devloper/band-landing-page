/* QuestionBlock.module.css */

.questionBlock {
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* Three equal columns */
  grid-template-rows: auto 1fr; /* Auto height for the question, equal height for sections */
  grid-template-areas:
    "question question question"
    "leftSection middleSection rightSection";
  grid-gap: 20px;
  margin: 20px 0;
  padding: 15px;
  border-radius: 8px;
}

.question {
  grid-area: question;
  text-align: center;
  font-size: 1.3rem;
  font-weight: bold;
  color: #2a2a2a;
  margin-bottom: 15px;
}

.responseSection {
  padding: 15px;
  font-size: 0.9rem;
  background-color: #f9f9f9;
  border-left: 2px solid #0073e6;
  border-radius: 6px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  overflow-y: auto;
  max-height: 200px;
}

.leftSection {
  grid-area: leftSection;
}

.middleSection {
  grid-area: middleSection;
}

.rightSection {
  grid-area: rightSection;
}

.responseList {
  margin: 10px 0;
  padding-left: 20px;
}

.responseItem {
  margin-bottom: 8px;
  font-size: 0.8rem;
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
  width: 100%;
  max-width: 300px;
  cursor: pointer;
  border-radius: 6px;
  margin-top: 10px;
  transition: transform 0.3s;
}

.responseImage:hover {
  transform: scale(1.05);
}








import React from 'react';
import styles from './ImageModal.module.css';

const ImageModal = ({ isOpen, closeModal, imageSrc, altText }) => {
  if (!isOpen) return null;

  return (
    <div className={styles.modalOverlay} onClick={closeModal}>
      <div className={styles.modalContent} onClick={(e) => e.stopPropagation()}>
        <img src={imageSrc} alt={altText} className={styles.modalImage} />
        <button onClick={closeModal} className={styles.closeButton}>X</button>
      </div>
    </div>
  );
};

export default ImageModal;
