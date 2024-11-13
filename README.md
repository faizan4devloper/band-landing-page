import React, { useState } from 'react';
import styles from './QuestionBlock.module.css';
import Modal from './ImageModal'; // Import the Modal component

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
      <div className={styles.imageWrapper}>
        
      <img
        src={content}
        alt={altText}
        className={styles.responseImage}
        onClick={() => handleImageClick(content)} // Open modal on image click
      />
      <span className={styles.tooltip}>Click to View Larger</span>
      </div>
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
