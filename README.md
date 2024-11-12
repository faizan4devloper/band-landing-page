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




import React from 'react';
import styles from './QuestionBlock.module.css';

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
      <img src={content} alt={altText} className={styles.responseImage} />
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
    </div>
  );
};

export default QuestionBlock;



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
