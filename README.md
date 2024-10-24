
import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import PropagateLoader from 'react-spinners/PropagateLoader'; // Import the spinner
import { faChevronDown, faChevronUp, faCaretDown, faCaretUp } from '@fortawesome/free-solid-svg-icons';

const MainContent = () => {
  const [contentData, setContentData] = useState([]);
    const [loading, setLoading] = useState(true); // New loading state
  const [openQuestion, setOpenQuestion] = useState(0); // Default first question open
  const [openGridItems, setOpenGridItems] = useState({
    textualResponse: true, // Textual Response open by default
    citizenExperience: false,
    factualInfo: false,
    contextual: false,
  });

  const fetchData = async () => {
    try {
      const response = await axios.post(
        'dummy',
        {
          question:
            'what are the average class sizes and student-teacher ratios in the local schools react?',
        },
        {
          headers: {
            'Content-Type': 'application/json',
          },
        }
      );

      const parsedResponse = JSON.parse(response.data.body);
      const llmAnswer = parsedResponse.answer;

      const formattedData = [
        {
          question:
            'What are the average class sizes and student-teacher ratios in the local schools?',
          answer: {
            textualResponse: llmAnswer || 'No Answer Available',
            citizenExperience: 'Citizen experience response goes here.',
            factualInfo: 'Factual information goes here.',
            contextual: 'Contextual information goes here.',
          },
        },
      ];

      setContentData(formattedData);
      setLoading(false)
    } catch (error) {
      console.error('Error fetching data:', error);
      setLoading(false)
    }
  };

  useEffect(() => {
    fetchData();
  }, []);

  const toggleAnswer = (index) => {
    if (openQuestion === index) {
      setOpenQuestion(null); // Collapse the question if clicked again
    } else {
      setOpenQuestion(index); // Open the new question
      // Expand only textualResponse for the new question
      setOpenGridItems({
        textualResponse: true, // Default expand Textual Response
        citizenExperience: false,
        factualInfo: false,
        contextual: false,
      });
    }
  };

  const toggleGridItem = (section) => {
    setOpenGridItems((prevState) => ({
      ...prevState,
      [section]: !prevState[section],
    }));
  };

  return (
     <div className={styles.mainContent}>
      {loading ? (
        <div className={styles.loaderWrapper}>
          {/* Spinner while data is loading */}
          <PropagateLoader color="rgb(15, 95, 220)" loading={loading} size={25} />
        </div>
      ) : Array.isArray(contentData) && contentData.length > 0 ? (
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
                {/* Textual Response expanded by default */}
                <div
                  className={`${styles.gridItem} ${
                    openGridItems.textualResponse
                      ? styles.expanded
                      : styles.collapsed
                  }`}
                  onClick={() => toggleGridItem('textualResponse')}
                >
                  <h3>Textual Response</h3>
                  <FontAwesomeIcon
                    icon={
                      openGridItems.textualResponse
                        ? faCaretUp
                        : faCaretDown
                    }
                    className={styles.chevronIcon}
                  />
                  {openGridItems.textualResponse && (
                    <p>{item.answer.textualResponse}</p>
                  )}
                </div>

                <div
                  className={`${styles.gridItem} ${
                    openGridItems.citizenExperience
                      ? styles.expanded
                      : styles.collapsed
                  }`}
                  onClick={() => toggleGridItem('citizenExperience')}
                >
                  <h3>Citizen Experience</h3>
                  <FontAwesomeIcon
                    icon={
                      openGridItems.citizenExperience
                        ? faCaretUp
                        : faCaretDown
                    }
                    className={styles.chevronIcon}
                  />
                  {openGridItems.citizenExperience && (
                    <p>{item.answer.citizenExperience}</p>
                  )}
                </div>

                <div
                  className={`${styles.gridItem} ${
                    openGridItems.factualInfo
                      ? styles.expanded
                      : styles.collapsed
                  }`}
                  onClick={() => toggleGridItem('factualInfo')}
                >
                  <h3>Factual Info</h3>
                  <FontAwesomeIcon
                    icon={
                      openGridItems.factualInfo ? faCaretUp : faCaretDown
                    }
                    className={styles.chevronIcon}
                  />
                  {openGridItems.factualInfo && (
                    <p>{item.answer.factualInfo}</p>
                  )}
                </div>

                <div
                  className={`${styles.gridItem} ${
                    openGridItems.contextual
                      ? styles.expanded
                      : styles.collapsed
                  }`}
                  onClick={() => toggleGridItem('contextual')}
                >
                  <h3>Contextual</h3>
                  <FontAwesomeIcon
                    icon={openGridItems.contextual ? faCaretUp : faCaretDown}
                    className={styles.chevronIcon}
                  />
                  {openGridItems.contextual && (
                    <p>{item.answer.contextual}</p>
                  )}
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
  font-size: 1em;
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
  margin-left: 10px;
  transition: transform 0.3s ease;
}

.gridAnswer {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); /* Independent columns */
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
  transition: padding 0.3s ease, max-height 0.3s ease; /* Smooth transitions */
  overflow: hidden;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  position: relative;
  min-height: 60px;
  max-height: 60px; /* Initially collapsed height */
}

.gridItem h3 {
  font-size: 1.2em;
  margin-left: 10px;
  margin-bottom: 8px;
  color: #333;
}

.gridItem p {
  font-size: 1em;
  color: #555;
  line-height: 1.6;
  margin: 0;
}

.expanded {
  max-height: 200px; /* Expanded height */
  overflow-y: auto;  /* Scroll when content exceeds max height */
}

.collapsed {
  max-height: 60px; /* Collapsed height */
  overflow: hidden;
}

.loaderWrapper{
  display: flex;
  justify-content: center;
  align-items: center;
  height: 60vh;
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

/* Adjust spacing between gridItems inside the gridAnswer */
.gridAnswer > .gridItem {
  margin-bottom: 15px;
}

/* Optional hover effect on gridItem for better UX */
.gridItem:hover {
  background-color: #f9f9f9;
}

/* Optional: Better visual feedback when the gridItem is expanded */
.gridItem.expanded:hover {
  background-color: #fff;
}
