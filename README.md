import React from 'react';
import styles from './QuestionBlock.module.css';

const QuestionBlock = ({ question, answerData }) => (
  <div className={styles.questionBlock}>
    <div className={styles.question}>
      {question}
    </div>
    <div className={styles.answer}>
      <p><strong>Textual Response:</strong> {answerData.textualResponse.join(', ')}</p>
      <p><strong>Citizen Experience:</strong> {answerData.citizenExperience}</p>
      <p><strong>Factual Info:</strong> {answerData.factualInfo}</p>
      <p><strong>Contextual:</strong> {answerData.contextual}</p>
    </div>
  </div>
);

export default QuestionBlock;


.questionBlock {
  padding: 10px; /* Increased padding for comfort */
  margin: 20px 0; /* Top and bottom margin for spacing from other elements */
  border-radius: 5px; /* Rounded corners */
  background-color: #ffffff; /* White background for clarity */
  transition: transform 0.2s; /* Smooth scaling effect on hover */
}



.question {
  font-size: 1.1rem; /* Larger font size for question text */
  font-weight: bold; /* Bold for emphasis */
  color: #2a2a2a; /* Dark text color for readability */
  margin-bottom: 10px; /* Space below question text */
}

.answer {
  font-size: .8rem; /* Normal font size for answers */
  color: #444; /* Dark grey for answers */
  margin-top: 10px; /* Space above answer text */
  padding: 10px; /* Padding around answer */
  background-color: #fafafa; /* Light background for answers */
  border-left: 4px solid #0073e6; /* Blue left border for visual interest */
  border-radius: 4px; /* Slight rounding for the answer block */
}
