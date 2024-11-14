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
        {arrayToRender.map((item, index) => {
          // Check if the item is a valid URL (specifically for PDFs)
          const isPdfLink = item.startsWith('http') && item.endsWith('.pdf');

          return (
            <li key={index} className={styles.responseItem}>
              {isPdfLink ? (
                // Mask the PDF link with "View PDF" text
                <a href={item} target="_blank" rel="noopener noreferrer" className={styles.link}>
                  View PDF
                </a>
              ) : (
                parseLinks(item)  // Use the existing parseLinks for other URLs
              )}
            </li>
          );
        })}
      </ul>
    );
  };

  const renderContent = (content, altText) => {
    // Check if the content exists and is a valid URL (image URL)
    if (content && content.startsWith('http')) {
      return (
        <div className={styles.imageWrapper}>
          <img
            src={content}
            alt={altText}
            className={styles.responseImage}
            onClick={() => handleImageClick(content)} // Open modal on image click
          />
          <span className={styles.tooltip}>Click to View Larger</span>
        </div>
      );
    } else {
      // If no image, display a beautiful message instead of an error or blank space
      return (
        <div className={styles.noImageMessage}>
          <p>{content ? "No image available for this content." : "No content available."}</p>
        </div>
      );
    }
  };

  // Masking the source link with 'Click here'
  const renderSourceLink = (source) => {
    if (source && source.startsWith("http")) {
      return (
        <a href={source} target="_blank" rel="noopener noreferrer" className={styles.link}>
          Click here
        </a>
      );
    } else {
      return 'No source available.';
    }
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
        {renderContent(answerData.citizenReview, "No Data Available")}
      </div>

      {/* Factual Info (Display as text or image) */}
      <div className={styles.responseSection}>
        <p><strong>Factual Content:</strong></p>
        {renderContent(answerData.factualData, "No Data Available")}
      </div>

      {/* Source Link Section with Masking */}
      <div className={styles.responseSection}>
        <p><strong>Source:</strong> {renderSourceLink(answerData.textualResponse[answerData.textualResponse.length - 1]?.match(/Source:\s*(https?:\/\/[^\s]+)/)?.[1])}</p>
      </div>

      {/* Modal for Image */}
      <Modal isOpen={isModalOpen} closeModal={closeModal} imageSrc={imageSrc} altText="Modal Image" />
    </div>
  );
};

export default QuestionBlock;
