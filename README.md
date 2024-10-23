import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown } from '@fortawesome/free-solid-svg-icons';

const MainContent = () => {
  const [contentData, setContentData] = useState([]);
  const [openQuestion, setOpenQuestion] = useState(null);

  // Fetch data from API using POST request
  const fetchData = async () => {
    try {
      // Sending the question as a query parameter
      const response = await axios.post('dummy', {
        question: 'what are the average class sizes and student-teacher ratios in the local schools react?'
      }, {
        headers: {
          'Content-Type': 'application/json'
        },
      });
      
      console.log('API Response:', response.data); // Log the API response

      // Parse the response body if it is a JSON string
      const parsedBody = JSON.parse(response.data.body);

      // Assuming 'parsedBody' contains the necessary answer structure
      setContentData([parsedBody]); // Wrapping in an array to fit map logic
    } catch (error) {
      console.error('Error fetching data:', error);
    }
  };

  useEffect(() => {
    fetchData();
  }, []);

  const toggleAnswer = (index) => {
    setOpenQuestion(openQuestion === index ? null : index);
  };

  return (
    <div className={styles.mainContent}>
      {Array.isArray(contentData) && contentData.length > 0 ? ( // Check if contentData is not empty
        contentData.map((item, index) => (
          <div key={index} className={styles.questionBlock}>
            <div
              className={styles.question}
              onClick={() => toggleAnswer(index)}
            >
              {/* Example: If the API doesn't return a 'question', adjust the key accordingly */}
              <span>Question {index + 1}</span>
              <FontAwesomeIcon
                icon={faChevronDown}
                className={styles.chevronIcon}
              />
            </div>
            {openQuestion === index && ( // Show answer only for the open question
              <div className={styles.answerBlock}>
                <div className={styles.gridAnswer}>
                  <div className={styles.gridItem}>
                    <h3>Textual Response</h3>
                    <p>{item.answer?.textual || 'No Textual Response Available'}</p>
                  </div>
                  <div className={styles.gridItem}>
                    <h3>Citizen Experience</h3>
                    <p>{item.answer?.citizenExperience || 'No Citizen Experience Available'}</p>
                  </div>
                  <div className={styles.gridItem}>
                    <h3>Factual Info</h3>
                    <p>{item.answer?.factualInfo || 'No Factual Info Available'}</p>
                  </div>
                  <div className={styles.gridItem}>
                    <h3>Contextual</h3>
                    <p>{item.answer?.contextual || 'No Contextual Info Available'}</p>
                  </div>
                </div>
              </div>
            )}
          </div>
        ))
      ) : (
        <div>No data available</div> // Handling case where contentData is empty
      )}
    </div>
  );
};

export default MainContent;







.mainContent {
  flex-grow: 1;
  height: 100vh;
  padding: 35px;
  padding-top: 60px;
  background-color: #f7f9fc;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.questionBlock {
  margin-bottom: 15px;
  border: 1px solid #ddd;
  border-radius: 8px;
  background-color: #fff;
  transition: background-color 0.3s ease;
}

.questionBlock:hover {
  background-color: #f1f1f1;
}

.question {
  font-size: 1.1em;
  padding: 10px;
  cursor: pointer;
  color: #333;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.chevronIcon {
  font-size: 1em;
  color: #888;
  transition: transform 0.3s ease;
}

/* Grid styling for the answer block */
.gridAnswer {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.gridItem {
  padding: 20px;
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease;
  max-height: 250px; /* Fixed height to limit long content */
  overflow-y: auto; /* Enable scrolling for overflow content */
  position: relative; /* Ensure inner elements are properly positioned */
}

.gridItem:hover {
  transform: translateY(-5px);
}

.gridItem h3 {
  font-size: 1.2em;
  margin-bottom: 10px;
  color: #333;
}

.gridItem p {
  font-size: 1em;
  color: #555;
  line-height: 1.6;
}

/* Optional: Add a subtle scrollbar style for better UI */
.gridItem::-webkit-scrollbar {
  width: 8px;
}

.gridItem::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: 8px;
}

.gridItem::-webkit-scrollbar-thumb {
  background-color: #888;
  border-radius: 8px;
  border: 2px solid #f1f1f1;
}

.gridItem::-webkit-scrollbar-thumb:hover {
  background-color: #555;
}

.answer {
  padding: 15px;
  background-color: #f8f9fa;
  font-size: 1em;
  line-height: 1.6;
  color: #555;
}
