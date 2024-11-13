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




/* Wrapper for the image */
.imageWrapper {
  position: relative;
  display: inline-block;
  cursor: pointer;
}

/* Tooltip styles */
.tooltip {
  visibility: hidden;
  position: absolute;
  bottom: 100%; /* Position the tooltip above the image */
  left: 50%;
  transform: translateX(-50%);
  background-color: rgba(0, 0, 0, 0.75);
  color: #fff;
  text-align: center;
  border-radius: 5px;
  padding: 5px;
  font-size: 0.9rem;
  opacity: 0;
  transition: opacity 0.3s, visibility 0s 0.3s; /* Smooth fade-in/out */
  z-index: 1;
}

/* Show tooltip on hover */
.imageWrapper:hover .tooltip {
  visibility: visible;
  opacity: 1;
  transition: opacity 0.3s, visibility 0s;
}

/* Image styles */
/*.responseImage {*/
/*  max-width: 100%;*/
/*  height: auto;*/
/*  border-radius: 8px;*/
/*  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);*/
/*}*/

/* You can also add hover effects for the image if needed */
.responseImage:hover {
  opacity: 0.8;
  transition: opacity 0.3s ease;
}

.questionBlock {
  display: grid;
  grid-template-areas:
    "question question"
    "leftSection rightSection"
    "bottomLeftSection bottomRightSection";
  grid-gap: 20px 40px;
  position: relative;
  margin: 20px 0;
  border-radius: 8px;
  padding: 15px;
  /*background-color: #ffffff;*/
  /*box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);*/
}

.question {
  grid-area: question;
  text-align: center;
  font-size: 1.3rem;
  font-weight: bold;
  color: #2a2a2a;
  margin-bottom: 15px;
}

strong{
  color:#572ac2;
}

.responseSection {
  padding: 15px;
  font-size: 0.8rem;
  background-color: #f9f9f9;
  border-left: 2px solid #0073e6;
  border-radius: 6px;
  position: relative;
  max-height: 200px;
  overflow-y: auto; /* Enable vertical scrolling */
  width: 100%;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: max-height 0.4s ease, padding 0.4s ease;
}

.responseSection::-webkit-scrollbar {
  width: 6px;
}

.responseSection::-webkit-scrollbar-thumb {
  background-color: rgba(15, 95, 220, 1);
  border-radius: 10px;
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
  font-size: 0.8rem;
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

.responseImage{
  max-width: 100%;
  height: auto;
  margin-top: 10px;
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
  font-size: 0.8rem;
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
