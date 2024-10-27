import React from 'react';
import styles from './QuestionBlock.module.css';
import { Line } from 'react-chartjs-2'; // Example for using Chart.js

const QuestionBlock = ({ question, answerData, graphData }) => (
  <div className={styles.questionBlock}>
    <div className={styles.question}>{question}</div>

    <div className={styles.answer}>
      <div className={styles.section}>
        <h3>Textual Response</h3>
        <p>{answerData.textualResponse.join(', ')}</p>
      </div>
      
      <div className={styles.section}>
        <h3>Citizen Experience</h3>
        <p>{answerData.citizenExperience}</p>
      </div>
      
      <div className={styles.section}>
        <h3>Factual Info</h3>
        <p>{answerData.factualInfo}</p>
      </div>
      
      <div className={styles.section}>
        <h3>Contextual</h3>
        <p>{answerData.contextual}</p>
      </div>

      {/* Optional: Display a graph if graphData is provided */}
      {graphData && (
        <div className={styles.graphContainer}>
          <h3>Data Visualization</h3>
          <Line data={graphData} options={{ responsive: true }} />
        </div>
      )}
    </div>
  </div>
);

export default QuestionBlock;


.questionBlock {
  padding: 20px; /* Increased padding for comfort */
  margin: 20px 0; /* Top and bottom margin for spacing from other elements */
  border-radius: 10px; /* More rounded corners */
  background-color: #ffffff; /* White background for clarity */
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1); /* Light shadow for depth */
  transition: transform 0.2s; /* Smooth scaling effect on hover */
}

.question {
  font-size: 1.5rem; /* Larger font size for question text */
  font-weight: bold; /* Bold for emphasis */
  color: #2a2a2a; /* Dark text color for readability */
  margin-bottom: 15px; /* Space below question text */
}

.answer {
  font-size: 1.1rem; /* Normal font size for answers */
  color: #444; /* Dark grey for answers */
  margin-top: 20px; /* Space above answer text */
}

.section {
  margin-bottom: 15px; /* Space between sections */
  padding: 15px; /* Padding around section */
  background-color: #fafafa; /* Light background for sections */
  border-left: 4px solid #0073e6; /* Blue left border for visual interest */
  border-radius: 5px; /* Slight rounding for section */
}

.graphContainer {
  margin-top: 20px; /* Space above the graph container */
  padding: 15px; /* Padding around graph */
  background-color: #f0f0f0; /* Slightly different background for graph */
  border-radius: 5px; /* Rounded corners for graph container */
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1); /* Light shadow for graph container */
}
