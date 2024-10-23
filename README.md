import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';

const MainContent = () => {
  const [contentData, setContentData] = useState([]);
  const [openQuestion, setOpenQuestion] = useState(null);

  const fetchData = async () => {
    try {
      const response = await axios.post('https://2kn1kfoouh.execute-api.us-east-1.amazonaws.com/edu/cit-adv2', {
        question: 'what are the average class sizes and student-teacher ratios in the local schools react?'
      }, {
        headers: {
          'Content-Type': 'application/json'
        },
      });

      const parsedResponse = JSON.parse(response.data.body);
      const llmAnswer = parsedResponse.answer;

      const formattedData = [
        {
          question: 'What are the average class sizes and student-teacher ratios in the local schools?',
          answer: llmAnswer || 'No Answer Available'
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
            {openQuestion === index && ( // Show answer only for the open question
              <div className={styles.answer}>
                <p>{item.answer}</p>
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



.mainContent {
  flex-grow: 1;
  height: 100vh;
  padding: 35px;
  padding-top: 60px;
  background-color: #f7f9fc;
  overflow-y: auto;
  width: 800px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  display: flex; /* Use flexbox to manage layout */
  flex-direction: column; /* Stack items vertically */
}

.questionBlock {
  margin-bottom: 15px;
  border: 1px solid #ddd;
  border-radius: 8px;
  background-color: #fff;
  transition: background-color 0.3s ease;
  overflow: hidden; /* Prevent content overflow */
}

.questionBlock:hover {
  background-color: #f1f1f1;
}

.question {
  font-size: 1.2em; /* Increased font size for better readability */
  padding: 15px; /* Adjusted padding */
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

.answer {
  padding: 20px; /* Increased padding for answer block */
  background-color: #f8f9fa; /* Light background for answer */
  font-size: 1em; /* Consistent font size */
  line-height: 1.6;
  color: #555;
}

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
}

.gridItem:hover {
  transform: translateY(-5px);
}

.gridItem h3 {
  font-size: 1.2em; /* Keep consistent font size */
  margin-bottom: 10px;
  color: #333;
}

.gridItem p {
  font-size: 1em; /* Keep consistent font size */
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

