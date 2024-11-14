

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

      {/* Modal for Image */}
      <Modal isOpen={isModalOpen} closeModal={closeModal} imageSrc={imageSrc} altText="Modal Image" />
    </div>
  );
};

export default QuestionBlock;







const fetchDataForQuestion = async (question) => {
  const payload = {
    question_id: question.id,
    question: question.text,
  };

  try {
    const response = await axios.post(
      'dummy', // Replace with your actual API endpoint
      payload,
      { headers: { 'Content-Type': 'application/json' } }
    );

    console.log('API Response:', response.data)

    // Check if `response.data.body` is already an object or a JSON string
    const parsedResponse = typeof response.data.body === 'string'
      ? JSON.parse(response.data.body)
      : response.data.body;

    console.log(parsedResponse)

    const llmAnswer = parsedResponse.answer || '';
    const formattedAnswer = llmAnswer.split('-').map(line => line.trim()).filter(line => line);

    const factualData = parsedResponse.factualData && parsedResponse.factualData.startsWith("http")
      ? parsedResponse.factualData
      : 'No factual information available.';
      
    const citizenReview = parsedResponse.citizenReview && parsedResponse.citizenReview.startsWith("http")
      ? parsedResponse.citizenReview
      : 'No citizen experience information available.';

    const Source = parsedResponse.Source || 'No source available.';

    // Mask the Source link into 'Click here'
    const maskedSource = Source.startsWith("http") 
      ? `<a href="${Source}" target="_blank" rel="noopener noreferrer">Click here</a>` 
      : 'No source available.';

    return {
      question: question.text,
      answer: {
        textualResponse: formattedAnswer.length > 0 ? [...formattedAnswer, `Source: ${maskedSource}`] : ['No Answer Available'],
        factualData: factualData,
        citizenReview: citizenReview,
        contextual: parsedResponse.contextual || 'No contextual information available.',
      },
    };
  } catch (error) {
    console.error("Error fetching data for question:", error);
    return {
      question: question.text,
      answer: {
        textualResponse: ['No Answer Available'],
        factualData: 'No factual information available.',
        citizenReview: 'No citizen experience information available.',
        contextual: 'No contextual information available.',
      },
    };
  }
};




const QuestionBlock = ({ question, answerData }) => {
  return (
    <div className={styles.questionBlock}>
      <h3>{question}</h3>
      <div>
        {answerData.textualResponse.map((line, index) => (
          <p key={index} dangerouslySetInnerHTML={{ __html: line }} />
        ))}
      </div>
    </div>
  );
};
