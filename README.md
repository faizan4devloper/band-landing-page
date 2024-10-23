import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';

const MainContent = () => {
  const [contentData, setContentData] = useState([]);
  const [openGridItems, setOpenGridItems] = useState({
    textualResponse: true,  // Default expanded
    citizenExperience: false,
    factualInfo: false,
    contextual: false
  });

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
            citizenExperience: 'Citizen Experience content here...',
            factualInfo: 'Factual Information content here...',
            contextual: 'Contextual Information content here...'
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

  const toggleGridItem = (item) => {
    setOpenGridItems((prevState) => ({
      ...prevState,
      [item]: !prevState[item],
    }));
  };

  return (
    <div className={styles.mainContent}>
      {Array.isArray(contentData) && contentData.length > 0 ? (
        contentData.map((item, index) => (
          <div key={index} className={styles.questionBlock}>
            {/* Textual Response - Default Expanded */}
            <div
              className={`${styles.gridItem} ${openGridItems.textualResponse ? styles.expanded : styles.collapsed}`}
              onClick={() => toggleGridItem('textualResponse')}
            >
              <h3>Textual Response</h3>
              <FontAwesomeIcon
                icon={openGridItems.textualResponse ? faChevronUp : faChevronDown}
                className={styles.chevronIcon}
              />
              {openGridItems.textualResponse && <p>{item.answer.textualResponse}</p>}
            </div>

            {/* Citizen Experience */}
            <div
              className={`${styles.gridItem} ${openGridItems.citizenExperience ? styles.expanded : styles.collapsed}`}
              onClick={() => toggleGridItem('citizenExperience')}
            >
              <h3>Citizen Experience</h3>
              <FontAwesomeIcon
                icon={openGridItems.citizenExperience ? faChevronUp : faChevronDown}
                className={styles.chevronIcon}
              />
              {openGridItems.citizenExperience && <p>{item.answer.citizenExperience}</p>}
            </div>

            {/* Factual Info */}
            <div
              className={`${styles.gridItem} ${openGridItems.factualInfo ? styles.expanded : styles.collapsed}`}
              onClick={() => toggleGridItem('factualInfo')}
            >
              <h3>Factual Info</h3>
              <FontAwesomeIcon
                icon={openGridItems.factualInfo ? faChevronUp : faChevronDown}
                className={styles.chevronIcon}
              />
              {openGridItems.factualInfo && <p>{item.answer.factualInfo}</p>}
            </div>

            {/* Contextual */}
            <div
              className={`${styles.gridItem} ${openGridItems.contextual ? styles.expanded : styles.collapsed}`}
              onClick={() => toggleGridItem('contextual')}
            >
              <h3>Contextual</h3>
              <FontAwesomeIcon
                icon={openGridItems.contextual ? faChevronUp : faChevronDown}
                className={styles.chevronIcon}
              />
              {openGridItems.contextual && <p>{item.answer.contextual}</p>}
            </div>
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
}

.questionBlock:hover {
  background-color: #f1f1f1;
}

.gridItem {
  padding: 15px;
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: height 0.3s ease, padding 0.3s ease;
  overflow: hidden;
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
}

.gridItem h3 {
  font-size: 1.2em;
  color: #333;
}

.gridItem p {
  font-size: 1em;
  color: #555;
  line-height: 1.6;
  margin-top: 10px;
}

.chevronIcon {
  font-size: 1.2em;
  color: #888;
}

.expanded {
  height: auto; /* Expanded height based on content */
}

.collapsed {
  height: 60px; /* Collapsed height */
  overflow: hidden;
}
