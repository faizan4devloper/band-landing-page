import React from 'react';
import styles from './Modal.module.css';

const Modal = ({ isOpen, closeModal, imageSrc, altText }) => {
  if (!isOpen) return null;

  return (
    <div className={styles.modalOverlay} onClick={closeModal}>
      <div className={styles.modalContent} onClick={(e) => e.stopPropagation()}>
        <img src={imageSrc} alt={altText} className={styles.modalImage} />
        <button onClick={closeModal} className={styles.closeButton}>
          Close
        </button>
      </div>
    </div>
  );
};

export default Modal;



/* Modal Overlay */
.modalOverlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

/* Modal Content */
.modalContent {
  background: white;
  padding: 20px;
  border-radius: 10px;
  text-align: center;
  position: relative;
}

.modalImage {
  max-width: 80%;
  max-height: 80vh;
  margin-bottom: 20px;
}

/* Close Button */
.closeButton {
  position: absolute;
  top: 10px;
  right: 10px;
  background: #572ac2;
  color: white;
  padding: 10px;
  border: none;
  border-radius: 50%;
  cursor: pointer;
}

.closeButton:hover {
  background: #572ac2;
}



import React, { useState } from 'react';
import styles from './QuestionBlock.module.css';
import Modal from './Modal'; // Import the Modal component

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
  const [isModalOpen, setIsModalOpen] = useState(false);
  const [imageSrc, setImageSrc] = useState(null);

  const handleImageClick = (src) => {
    setImageSrc(src);
    setIsModalOpen(true); // Open the modal
  };

  const closeModal = () => {
    setIsModalOpen(false); // Close the modal
    setImageSrc(null); // Reset the image source
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

  const renderContent = (content, altText) => {
    // Display as an image if content is a URL; otherwise, treat as text
    return content && content.startsWith('http') ? (
      <img
        src={content}
        alt={altText}
        className={styles.responseImage}
        onClick={() => handleImageClick(content)} // Open modal on image click
      />
    ) : (
      <p>{content}</p>
    );
  };

  return (
    <div className={styles.questionBlock}>
      <div className={styles.question}>{question}</div>

      {/* Textual Response */}
      <div className={styles.responseSection}>
        <p><strong>School Sources Insights:</strong></p>
        {renderResponsePoints(answerData.textualResponse)}
      </div>

      {/* Citizen Experience (Display as text or image) */}
      <div className={styles.responseSection}>
        <p><strong>Citizen Speak:</strong></p>
        {renderContent(answerData.citizenReview, "Citizen Experience")}
      </div>

      {/* Factual Info (Display as text or image) */}
      <div className={styles.responseSection}>
        <p><strong>Factual Content:</strong></p>
        {renderContent(answerData.factualData, "Factual Info")}
      </div>

      {/* Modal for Image */}
      <Modal isOpen={isModalOpen} closeModal={closeModal} imageSrc={imageSrc} altText="Modal Image" />
    </div>
  );
};

export default QuestionBlock;
