import React, { useState, useEffect } from 'react';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';

const MainContent = () => {
  const [contentData, setContentData] = useState([]);
  const [openQuestion, setOpenQuestion] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  // Function to fetch data from the API
  const fetchData = async () => {
    try {
      const response = await fetch('https://dummy', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          question: 'what are the averages?',
        }),
      });

      if (!response.ok) {
        throw new Error('Network response was not ok');
      }

      const data = await response.json();
      setContentData(data);  // Assuming the API returns the array of questions and answers
      setLoading(false);
    } catch (error) {
      setError(error.message);
      setLoading(false);
    }
  };

  // Fetch data when the component mounts
  useEffect(() => {
    fetchData();
  }, []);

  const toggleAnswer = (index) => {
    setOpenQuestion(openQuestion === index ? null : index);
  };

  // If loading or error states occur
  if (loading) {
    return <div>Loading...</div>;
  }

  if (error) {
    return <div>Error: {error}</div>;
  }

  return (
    <div className={styles.mainContent}>
      {openQuestion === null ? (
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
          {/* Display grid layout for the first question if answer is an object */}
          {typeof contentData[openQuestion].answer === 'object' ? (
            <div className={styles.gridAnswer}>
              <div className={styles.gridItem}>
                <h3>Textual Response</h3>
                <p>{contentData[openQuestion].answer.textual}</p>
              </div>
              <div className={styles.gridItem}>
                <h3>Citizen Experience</h3>
                <p>{contentData[openQuestion].answer.citizenExperience}</p>
              </div>
              <div className={styles.gridItem}>
                <h3>Factual Info</h3>
                <p>{contentData[openQuestion].answer.factualInfo}</p>
              </div>
              <div className={styles.gridItem}>
                <h3>Contextual</h3>
                <p>{contentData[openQuestion].answer.contextual}</p>
              </div>
            </div>
          ) : (
            <div className={styles.answer}>
              {contentData[openQuestion].answer}
            </div>
          )}
        </div>
      )}
    </div>
  );
};

export default MainContent;
