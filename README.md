import React from 'react';
import styles from './QuestionBlock.module.css';

// Utility to parse text for URLs and convert them to clickable links
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
  if (!answerData) return null;

  return (
    <div className={styles.questionBlock}>
      {/* Display the selected question */}
      <div className={styles.question}>
        <strong>Question:</strong> {question}
      </div>

      {/* Textual Response Section */}
      {answerData.textualResponse && answerData.textualResponse.length > 0 && (
        <div className={styles.responseSection}>
          <p><strong>Textual Response:</strong></p>
          <ul>
            {answerData.textualResponse.map((item, index) => (
              <li key={index}>{parseLinks(item)}</li>
            ))}
          </ul>
        </div>
      )}

      {/* Factual Data Section */}
      {answerData.factualData && (
        <div className={styles.factualSection}>
          <p><strong>Factual Data:</strong></p>
          <table className={styles.dataTable}>
            <thead>
              <tr>
                <th>Data Point</th>
                <th>Value</th>
              </tr>
            </thead>
            <tbody>
              {Object.entries(answerData.factualData).map(([key, value], index) => (
                <tr key={index}>
                  <td>{key}</td>
                  <td>{value}</td>
                </tr>
              ))}
            </tbody>
          </table>
        </div>
      )}

      {/* Citizen Review Section */}
      {answerData.citizenReview && (
        <div className={styles.reviewSection}>
          <p><strong>Citizen Review:</strong></p>
          <p>{parseLinks(answerData.citizenReview)}</p>
        </div>
      )}

      {/* Contextual Information Section */}
      {answerData.contextual && (
        <div className={styles.contextualSection}>
          <p><strong>Contextual Information:</strong></p>
          <p>{parseLinks(answerData.contextual)}</p>
        </div>
      )}
    </div>
  );
};

export default QuestionBlock;
