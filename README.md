import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';

const MainContent = () => {
  const [contentData, setContentData] = useState([]);
  const [openQuestion, setOpenQuestion] = useState(null);

  // Fetch data from API using POST request
  const fetchData = async () => {
    try {
      const response = await axios.post('dummy', {
        question: 'what are the average class sizes and student-teacher ratios in the local schools react?'
      }, {
        headers: {
          'Content-Type': 'application/json'
        },
      });
      
      console.log('API Response:', response.data); // Log the API response
      // Assuming response.data.answer is a string containing the answer.
      const formattedData = [
        {
          question: 'What are the average class sizes and student-teacher ratios in the local schools?',
          answer: response.data.answer // Update this based on the structure of your API response
        }
      ];
      setContentData(formattedData);
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
      {Array.isArray(contentData) && contentData.length > 0 ? (
        contentData.map((item, index) => (
          <div key={index} className={styles.questionBlock}>
            <div
              className={styles.question}
              onClick={() => toggleAnswer(index)}
            >
              {item.question}
              <FontAwesomeIcon
                icon={openQuestion === index ? faChevronUp : faChevronDown}
                className={styles.chevronIcon}
              />
            </div>
            {openQuestion === index && (
              <div className={styles.answerBlock}>
                <p>{item.answer || 'No Answer Available'}</p>
              </div>
            )}
          </div>
        ))
      ) : (
        <div>No data available</div>
      )}
    </div>
  );
};

export default MainContent;
