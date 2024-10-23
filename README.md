import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';

// Use CORS Anywhere proxy for development (remove or change for production)
const proxyUrl = 'https://cors-anywhere.herokuapp.com/';
const apiUrl = 'https://dummy'; // Replace this with your actual API endpoint

const MainContent = () => {
  const [contentData, setContentData] = useState([]);
  const [openQuestion, setOpenQuestion] = useState(null);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        // Append the proxy URL to your API URL to avoid CORS issues in development
        const response = await axios.get(`${proxyUrl}${apiUrl}`, {
          params: {
            question: 'what are the averages?',
          },
        });
        setContentData(response.data); // Assume response.data is your contentData
      } catch (error) {
        console.error('Error fetching data:', error);
        setError('Failed to load data. Please try again later.');
      }
    };

    fetchData();
  }, []);

  const toggleAnswer = (index) => {
    setOpenQuestion(openQuestion === index ? null : index);
  };

  return (
    <div className={styles.mainContent}>
      {error && <div className={styles.error}>{error}</div>}
      {contentData.length > 0 ? (
        openQuestion === null ? (
          contentData.map((item, index) => (
            <div key={index} className={styles.questionBlock}>
              <div
                className={styles.question}
                onClick={() => toggleAnswer(index)}
              >
                {item.question}
                <FontAwesomeIcon
                  icon={faChevronDown}
                  className={styles.chevronIcon}
                />
              </div>
            </div>
          ))
        ) : (
          <div className={styles.questionBlock}>
            <div
              className={styles.question}
              onClick={() => toggleAnswer(openQuestion)}
            >
              {contentData[openQuestion].question}
              <FontAwesomeIcon
                icon={faChevronUp}
                className={styles.chevronIcon}
              />
            </div>
            {/* Display grid layout for the first question */}
            {openQuestion === 0 ? (
              <div className={styles.gridAnswer}>
                <div className={styles.gridItem}>
                  <h3>Textual Response</h3>
                  <p>{contentData[0].answer.textual}</p>
                </div>
                <div className={styles.gridItem}>
                  <h3>Citizen Experience</h3>
                  <p>{contentData[0].answer.citizenExperience}</p>
                </div>
                <div className={styles.gridItem}>
                  <h3>Factual Info</h3>
                  <p>{contentData[0].answer.factualInfo}</p>
                </div>
                <div className={styles.gridItem}>
                  <h3>Contextual</h3>
                  <p>{contentData[0].answer.contextual}</p>
                </div>
              </div>
            ) : (
              <div className={styles.answer}>
                {contentData[openQuestion].answer}
              </div>
            )}
          </div>
        )
      ) : (
        <div>Loading...</div>
      )}
    </div>
  );
};

export default MainContent;
