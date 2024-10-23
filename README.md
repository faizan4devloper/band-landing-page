import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';

const MainContent = () => {
  const [contentData, setContentData] = useState([]);

  // Fetch data from API using POST request
  const fetchData = async () => {
    try {
      const response = await axios.post('dummy', {
        question: 'what are the average class sizes and student-teacher ratios in the local schools?'
      }, {
        headers: {
          'Content-Type': 'application/json'
        },
      });

      console.log('API Response:', response.data); // Log the API response
      setContentData(response.data);
    } catch (error) {
      console.error('Error fetching data:', error);
    }
  };

  useEffect(() => {
    fetchData();
  }, []);

  return (
    <div className={styles.mainContent}>
      {Array.isArray(contentData) && contentData.length > 0 ? (
        contentData.map((item, index) => (
          <div key={index} className={styles.answerBlock}>
            <h3 className={styles.answerTitle}>Answer {index + 1}</h3>
            <p className={styles.answerText}>
              {item.answer?.textual || 'No Textual Response Available'}
            </p>
            <p className={styles.answerText}>
              {item.answer?.citizenExperience || 'No Citizen Experience Available'}
            </p>
            <p className={styles.answerText}>
              {item.answer?.factualInfo || 'No Factual Info Available'}
            </p>
            <p className={styles.answerText}>
              {item.answer?.contextual || 'No Contextual Info Available'}
            </p>
          </div>
        ))
      ) : (
        <div className={styles.noData}>No data available</div>
      )}
    </div>
  );
};

export default MainContent;
