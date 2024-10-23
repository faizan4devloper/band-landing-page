import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';

const MainContent = () => {
  const [contentData, setContentData] = useState([]);
  const [openQuestion, setOpenQuestion] = useState(0); // Default to first question open
  const [openGridItems, setOpenGridItems] = useState({}); // State to track open grid items

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

      const formattedData = [
        {
          question: 'What are the average class sizes and student-teacher ratios in the local schools?',
          answer: {
            textualResponse: llmAnswer || 'No Answer Available',
            citizenExperience: 'Citizen experience response goes here.',
            factualInfo: 'Factual information goes here.',
            contextual: 'Contextual information goes here.'
          }
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
    setOpenQuestion(openQuestion === index ? null : index); // Collapse if same question is clicked
  };

  const toggleGridItem = (section) => {
    setOpenGridItems(prevState => ({
      ...prevState,
      [section]: !prevState[section]
    }));
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
                <div
                  className={`${styles.gridItem} ${
                    openGridItems.textualResponse ? '' : styles.collapsed
                  }`}
                  onClick={() => toggleGridItem('textualResponse')}
                >
                  <h3>Textual Response</h3>
                  {openGridItems.textualResponse && <p>{item.answer.textualResponse}</p>}
                </div>

                <div
                  className={`${styles.gridItem} ${
                    openGridItems.citizenExperience ? '' : styles.collapsed
                  }`}
                  onClick={() => toggleGridItem('citizenExperience')}
                >
                  <h3>Citizen Experience</h3>
                  {openGridItems.citizenExperience && <p>{item.answer.citizenExperience}</p>}
                </div>

                <div
                  className={`${styles.gridItem} ${
                    openGridItems.factualInfo ? '' : styles.collapsed
                  }`}
                  onClick={() => toggleGridItem('factualInfo')}
                >
                  <h3>Factual Info</h3>
                  {openGridItems.factualInfo && <p>{item.answer.factualInfo}</p>}
                </div>

                <div
                  className={`${styles.gridItem} ${
                    openGridItems.contextual ? '' : styles.collapsed
                  }`}
                  onClick={() => toggleGridItem('contextual')}
                >
                  <h3>Contextual</h3>
                  {openGridItems.contextual && <p>{item.answer.contextual}</p>}
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
  max-height: 60px; /* Collapsed height */
  overflow: hidden;
  cursor: pointer;
}

.gridItem.expanded {
  max-height: 250px; /* Expanded height */
  overflow-y: auto;
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

.collapsed {
  max-height: 60px; /* Keep collapsed by default */
}

.gridItem:hover {
  transform: translateY(-5px);
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
