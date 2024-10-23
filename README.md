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



