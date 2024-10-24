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
                  className={${styles.gridItem} ${
                    openGridItems.textualResponse
                      ? styles.expanded
                      : styles.collapsed
                  }}
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
                  className={${styles.gridItem} ${
                    openGridItems.citizenExperience
                      ? styles.expanded
                      : styles.collapsed
                  }}
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
                  className={${styles.gridItem} ${
                    openGridItems.factualInfo
                      ? styles.expanded
                      : styles.collapsed
                  }}
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
                  className={${styles.gridItem} ${
                    openGridItems.contextual
                      ? styles.expanded
                      : styles.collapsed
                  }}
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
