Source: https
answer: etc
citizenReview
factualData
question
question_id


import React, { useState, useEffect } from 'react';
import axios from 'axios';
import PropagateLoader from 'react-spinners/PropagateLoader';
import FaqDropdown from './FaqDropdown';
import QuestionBlock from './QuestionBlock';
import styles from './MainContent.module.css';

const MainContent = ({ activeTopic }) => {
  const [questions, setQuestions] = useState([]); // Dynamically loaded questions for the topic
  const [contentData, setContentData] = useState([]); // Stores answers for each question
  const [loading, setLoading] = useState(true);
  const [selectedData, setSelectedData] = useState({});

  // Define the questions based on the active topic
  const topicQuestions = {
    1: [
      { id: 'Q1', text: 'What are the last five years key statistics for Serpell Primary School?' },
      { id: 'Q2', text: 'What are the admission criteria and process for Serpell Primary School?' },
      { id: 'Q3', text: 'How does the school perform in standardized tests and assessments?' },
    ],
    2: [
      { id: 'Q4', text: 'What’s the curriculum at Serpell Primary School?' },
      { id: 'Q5', text: 'How is the curriculum structured across different year levels?' },
      { id: 'Q6', text: 'What specialist programs are offered at Serpell Primary School?' },
    ],
    3: [
      { id: 'Q7', text: 'How does the school engage with the broader community?' },
      { id: 'Q8', text: 'How can parents get involved in the school community?' },
      { id: 'Q9', text: 'What support services are available for students with special needs at Serpell Primary School?' },
    ],
  };

 
  
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
      
        console.log('Factual Image URL:', factualData);  // Add this line
      

    const citizenReview = parsedResponse.citizenReview && parsedResponse.citizenReview.startsWith("http")
      ? parsedResponse.citizenReview
      : 'No citizen experience information available.';

    return {
      question: question.text,
      answer: {
        textualResponse: formattedAnswer.length > 0 ? formattedAnswer : ['No Answer Available'],
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


  // Fetch all data for the selected topic
  const fetchAllData = async (topicId) => {
    setLoading(true);
    try {
      const questionsList = topicQuestions[topicId] || [];
      const formattedData = await Promise.all(
        questionsList.map(fetchDataForQuestion)
      );
      setContentData(formattedData);
    } catch (error) {
      console.error("Error fetching data for all questions:", error);
    } finally {
      setLoading(false);
    }
  };

  const handleQuestionSelect = (question, answer) => {
    setSelectedData((prev) => ({
      ...prev,
      [activeTopic]: {
        question,
        answer,
      },
    }));
  };

  useEffect(() => {
    fetchAllData(activeTopic);
    setSelectedData((prev) => ({ ...prev, [activeTopic]: null }));
  }, [activeTopic]);

  const selectedQuestionData = selectedData[activeTopic] || {};

  return (
    <div className={styles.mainContent}>
      {loading ? (
        <div className={styles.loaderWrapper}>
          <PropagateLoader color="rgb(15, 95, 220)" loading={loading} size={22} />
        </div>
      ) : (
        <>
          <FaqDropdown
            contentData={contentData}
            onQuestionSelect={handleQuestionSelect}
            selectedQuestion={selectedQuestionData.question}
            selectedAnswer={selectedQuestionData.answer}
          />
          {selectedQuestionData.question && selectedQuestionData.answer && (
            <div className={styles.selectedQuestionBlock}>
              <QuestionBlock
                question={selectedQuestionData.question}
                answerData={selectedQuestionData.answer}
              />
            </div>
          )}
        </>
      )}
    </div>
  );
};

export default MainContent;



// import React from 'react';
// import styles from './QuestionBlock.module.css';

// // Utility function to convert URLs in text to anchor tags
// const parseLinks = (text) => {
//   const urlPattern = /(https?:\/\/[^\s]+)/g;
//   return text.split(urlPattern).map((part, index) =>
//     urlPattern.test(part) ? (
//       <a key={index} href={part} target="_blank" rel="noopener noreferrer" className={styles.link}>
//         {part}
//       </a>
//     ) : (
//       part
//     )
//   );
// };

// const QuestionBlock = ({ question, answerData }) => {
//   const renderResponsePoints = (responseArray) => {
//     const arrayToRender = Array.isArray(responseArray) ? responseArray : [responseArray];
//     return (
//       <ul className={styles.responseList}>
//         {arrayToRender.map((item, index) => (
//           <li key={index} className={styles.responseItem}>
//             {parseLinks(item)}
//           </li>
//         ))}
//       </ul>
//     );
//   };

//   const renderImage = (imageUrl, altText) =>
//     imageUrl ? (
//       <img src={imageUrl} alt={altText} className={styles.responseImage} />
//     ) : null;

//   return (
//     <div className={styles.questionBlock}>
//       <div className={styles.question}>{question}</div>

//       {/* Textual Response */}
//       <div className={styles.responseSection}>
//         <p><strong>Textual Response:</strong></p>
//         {renderResponsePoints(answerData.textualResponse)}
//         {renderImage(answerData.textualImage, "Textual Response Image")}
//       </div>

//       {/* Citizen Experience */}
//       <div className={styles.responseSection}>
//         <p><strong>Citizen Experience:</strong></p>
//         {renderImage(answerData.citizenReview, "Citizen Experience Image")}
//       </div>

//       {/* Factual Info */}
//       <div className={styles.responseSection}>
//         <p><strong>Factual Info:</strong></p>
//         {renderImage(answerData.factualData, "Factual Info Image")}
//       </div>

      
//     </div>
//   );
// };

// export default QuestionBlock;




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
