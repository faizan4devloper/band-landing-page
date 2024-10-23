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
      const response = await axios.post('dummy', {
        question: 'what are the average class sizes and student-teacher ratios in the local schools react?'
      }, {
        headers: {
          'Content-Type': 'application/json'
        },
      });

      const parsedResponse = JSON.parse(response.data.body);
      const llmAnswer = parsedResponse.answer;

      // Assuming different response types
      const formattedData = [
        {
          question: 'What are the average class sizes and student-teacher ratios in the local schools?',
          textualResponse: llmAnswer || 'No textual response available',
          citizenExperience: 'This is a citizen experience related to the question',
          factualInfo: 'This is factual information about the question',
          contextualInfo: 'This is contextual information about the question'
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
              <div className={styles.gridAnswer}>
                <div className={styles.gridItem}>
                  <h3>Textual Response</h3>
                  <p>{item.textualResponse}</p>
                </div>
                <div className={styles.gridItem}>
                  <h3>Citizen Experience</h3>
                  <p>{item.citizenExperience}</p>
                </div>
                <div className={styles.gridItem}>
                  <h3>Factual Info</h3>
                  <p>{item.factualInfo}</p>
                </div>
                <div className={styles.gridItem}>
                  <h3>Contextual Info</h3>
                  <p>{item.contextualInfo}</p>
                </div>
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
  display: flex;
  flex-direction: column;
}

.questionBlock {
  margin-bottom: 15px;
  border: 1px solid #ddd;
  border-radius: 8px;
  background-color: #fff;
  transition: background-color 0.3s ease;
  overflow: hidden;
}

.questionBlock:hover {
  background-color: #f1f1f1;
}

.question {
  font-size: 1.2em;
  padding: 15px;
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
  max-height: 250px;
  overflow-y: auto;
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
