
import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import PropagateLoader from 'react-spinners/PropagateLoader';
import { faChevronDown, faChevronUp, faCaretDown, faCaretUp } from '@fortawesome/free-solid-svg-icons';

const MainContent = ({ activeTopic }) => {
  const [contentData, setContentData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [openQuestion, setOpenQuestion] = useState(0); // First question expanded by default
  const [openGridItems, setOpenGridItems] = useState({
    textualResponse: true,
  });

  const topicQuestions = {
    1: [
      'What are the average class sizes and student-teacher ratios in the local schools?',
      'What are the admission criteria for the schools in this area?',
    ],
    2: ['Topic 2 Question 1', 'Topic 2 Question 2'],
    3: ['Topic 3 Question 1', 'Topic 3 Question 2'],
    4: ['Topic 4 Question 1', 'Topic 4 Question 2'],
    5: ['Topic 5 Question 1', 'Topic 5 Question 2'],
  };

  const fetchDataForQuestion = async (question) => {
    try {
      const response = await axios.post(
        'dummy', // Replace 'dummy' with your actual API endpoint
        { question: `${question}:- hi` },
        { headers: { 'Content-Type': 'application/json' } }
      );

      const parsedResponse = JSON.parse(response.data.body);
      const llmAnswer = parsedResponse.answer;
      const formattedAnswer = llmAnswer.split('-').map((line) => line.trim()).filter(line => line);

      return formattedAnswer.length > 0 ? formattedAnswer : ['No Answer Available'];
    } catch (error) {
      console.error('Error fetching data:', error);
      return ['No Answer Available'];
    }
  };

  const fetchAllData = async (topicId) => {
    try {
      const questionsList = topicQuestions[topicId] || [];
      const formattedData = await Promise.all(
        questionsList.map(async (question) => {
          const answer = await fetchDataForQuestion(question);
          return {
            question,
            answer: {
              textualResponse: answer,
              citizenExperience: 'Citizen experience response goes here.',
              factualInfo: 'Factual information goes here.',
              contextual: 'Contextual information goes here.',
            },
          };
        })
      );

      setContentData(formattedData);
      setLoading(false);
    } catch (error) {
      console.error('Error fetching data for all questions:', error);
      setLoading(false);
    }
  };

  useEffect(() => {
    setLoading(true);
    fetchAllData(activeTopic);
  }, [activeTopic]);

  const toggleAnswer = (index) => {
    setOpenQuestion(prevIndex => (prevIndex === index ? null : index)); // Toggle the selected question
    setOpenGridItems({
      textualResponse: true, // Textual response always expanded initially
    });
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
          <PropagateLoader color="rgb(15, 95, 220)" loading={loading} size={22} />
        </div>
      ) : Array.isArray(contentData) && contentData.length > 0 ? (
        contentData.map((item, index) => (
          <div key={index} className={styles.questionBlock}>
            <div className={styles.question} onClick={() => toggleAnswer(index)}>
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
                    openGridItems.textualResponse ? styles.expanded : styles.collapsed
                  }`}
                  onClick={() => toggleGridItem('textualResponse')}
                >
                  <h3>Textual Response</h3>
                  <FontAwesomeIcon
                    icon={openGridItems.textualResponse ? faCaretUp : faCaretDown}
                    className={styles.chevronIcon}
                  />
                  {openGridItems.textualResponse && (
                    <ul className={styles.answerList}>
                      {item.answer.textualResponse.map((line, idx) => (
                        <li key={idx}>- {line}</li>
                      ))}
                    </ul>
                  )}
                </div>

                <div
                  className={`${styles.gridItem} ${
                    openGridItems.citizenExperience ? styles.expanded : styles.collapsed
                  }`}
                  onClick={() => toggleGridItem('citizenExperience')}
                >
                  <h3>Citizen Experience</h3>
                  <FontAwesomeIcon
                    icon={openGridItems.citizenExperience ? faCaretUp : faCaretDown}
                    className={styles.chevronIcon}
                  />
                  {openGridItems.citizenExperience && (
                    <ul className={styles.answerList}>
                      <li>{item.answer.citizenExperience}</li>
                    </ul>
                  )}
                </div>

                <div
                  className={`${styles.gridItem} ${
                    openGridItems.factualInfo ? styles.expanded : styles.collapsed
                  }`}
                  onClick={() => toggleGridItem('factualInfo')}
                >
                  <h3>Factual Info</h3>
                  <FontAwesomeIcon
                    icon={openGridItems.factualInfo ? faCaretUp : faCaretDown}
                    className={styles.chevronIcon}
                  />
                  {openGridItems.factualInfo && (
                    <ul className={styles.answerList}>
                      <li>{item.answer.factualInfo}</li>
                    </ul>
                  )}
                </div>

                <div
                  className={`${styles.gridItem} ${
                    openGridItems.contextual ? styles.expanded : styles.collapsed
                  }`}
                  onClick={() => toggleGridItem('contextual')}
                >
                  <h3>Contextual</h3>
                  <FontAwesomeIcon
                    icon={openGridItems.contextual ? faCaretUp : faCaretDown}
                    className={styles.chevronIcon}
                  />
                  {openGridItems.contextual && (
                    <ul className={styles.answerList}>
                      <li>{item.answer.contextual}</li>
                    </ul>
                  )}
                </div>
              </div>
            )}
          </div>
        ))
      ) : (
        <div>No Questions Available</div>
      )}
    </div>
  );
};

export default MainContent;
