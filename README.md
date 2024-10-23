
import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css'; // Assuming your CSS module for styling
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';

const MainContent = () => {
  const [contentData, setContentData] = useState([]);
  const [openQuestion, setOpenQuestion] = useState(null);
      // const [userInput, setUserInput] = useState("what are the average class sizes and student-teacher ratios in the local schools?");


  // Fetch data from API using GET request
  const fetchData = async () => {
                const newMessage = 'what are the average class sizes and student-teacher ratios in the local schools?';

    try {
      const response = await axios.get('https://2kn1kfoouh.execute-api.us-east-1.amazonaws.com/edu/cit-adv2', {
       
                          question: newMessage.text,

headers: {
                        'Content-Type': 'application/json'
                  },
      });
      setContentData(response.data); // Assuming the API response is structured as expected
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
          {/* Display grid layout for the first question with multiple layouts */}
          {openQuestion === 0 ? (
            <div className={styles.gridAnswer}>
              <div className={styles.gridItem}>
                <h3>Textual Response</h3>
                <p>{contentData[0]?.answer?.textual || 'No Textual Response Available'}</p>
              </div>
              <div className={styles.gridItem}>
                <h3>Citizen Experience</h3>
                <p>{contentData[0]?.answer?.citizenExperience || 'No Citizen Experience Available'}</p>
              </div>
              <div className={styles.gridItem}>
                <h3>Factual Info</h3>
                <p>{contentData[0]?.answer?.factualInfo || 'No Factual Info Available'}</p>
              </div>
              <div className={styles.gridItem}>
                <h3>Contextual</h3>
                <p>{contentData[0]?.answer?.contextual || 'No Contextual Info Available'}</p>
              </div>
            </div>
          ) : (
            <div className={styles.answer}>
              {contentData[openQuestion].answer || 'No answer available'}
            </div>
          )}
        </div>
      )}
    </div>
  );
};

export default MainContent;
