
import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import PropagateLoader from 'react-spinners/PropagateLoader';
import { faChevronDown, faChevronUp, faCaretDown, faCaretUp } from '@fortawesome/free-solid-svg-icons';

const MainContent = ({ activeTopic }) => {
  const [contentData, setContentData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [openQuestion, setOpenQuestion] = useState(0); // Initially no question expanded
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
        'https://2kn1kfoouh.execute-api.us-east-1.amazonaws.com/edu/cit-adv2', 
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
      textualResponse: true, 
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
          (openQuestion === null || openQuestion === index) && ( // Only display either all or the expanded question
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
          )
        ))
      ) : (
        <div>No Questions Available</div>
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
  border-radius: 10px;
  background-color: #ffffff;
  transition: background-color 0.3s ease, box-shadow 0.3s ease;
  overflow: hidden;
}

.questionBlock:hover {
  background-color: #f0f0f0;
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.1);
}

.question {
  font-size: 1rem;
  font-weight: bold;
  padding: 14px;
  cursor: pointer;
  color: #2a2a2a;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: color 0.3s ease;
}

.question:hover {
  color: #0073e6;
}

.chevronIcon {
  font-size: 1rem;
  color: #888;
  margin-left: 10px;
  transition: transform 0.3s ease, color 0.3s ease;
}

.gridAnswer {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  max-height: 300px;
  overflow-y: auto;
  margin-top: 10px;
  gap: 20px;
  background-color: #eff0f7;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}


.answerList {
  padding-left: 20px;
  list-style: none;
  font-size: 1em;
  color: #555;
}

.answerList li {
  margin-bottom: 10px;
  line-height: 1.6;
  font-size: .9rem;
  font-weight: 500;
  color: #333;
}


.gridItem {
  padding: 25px 10px;
  background-color: #fff;
  border-radius: 12px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  min-height: 80px;
  max-height: 80px;
  overflow: hidden;
  position: relative;
  text-align: center;
}

.gridItem h3 {
  font-size: 1.4em;
  color: #2c3e50;
  font-weight: 600;
  margin-bottom: 10px;
}

.gridItem p {
  font-size: 1em;
  color: #7f8c8d;
  line-height: 1.6;
  margin: 0;
}

.gridItem:hover {
  /*background-color: #ecf0f1;*/
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
  transform: translateY(-3px);
}

.gridItem h3:hover {
  color: #0073e6;
}

.expanded {
  max-height: 250px;
  overflow-y: auto;
}

.collapsed {
  max-height: 80px;
  overflow: hidden;
}

/* Custom scrollbar */
.gridAnswer::-webkit-scrollbar{
  width: 8px;
}

.gridAnswer::-webkit-scrollbar-track {
  background: #f1f1f1;
}

.gridAnswer::-webkit-scrollbar-thumb {
  background-color: #888;
  border-radius: 8px;
  border: 2px solid #f1f1f1;
}

.gridAnswer::-webkit-scrollbar-thumb:hover {
  background-color: #555;
}





.gridItem::-webkit-scrollbar {
  width: 8px;
}

.gridItem::-webkit-scrollbar-track {
  background: #f1f1f1;
}

.gridItem::-webkit-scrollbar-thumb {
  background-color: #888;
  border-radius: 8px;
  border: 2px solid #f1f1f1;
}

.gridItem::-webkit-scrollbar-thumb:hover {
  background-color: #555;
}

.loaderWrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 60vh;
}

ul {
  /*list-style-type: disc;*/
  padding-left: 20px;
}

li {
  font-size: 1em;
  color: #444;
}

.gridAnswer > .gridItem {
  margin-bottom: 20px;
}

/* Additional hover effect for grid items */
.gridItem:hover {
  /*background: linear-gradient(135deg, #74ebd5 0%, #acb6e5 100%);*/
  /*color: white;*/
}

.gridItem:hover h3, .gridItem:hover p {
  /*color: #fff;*/  
}

/* Optional hover effect for better UI feedback when grid item is expanded */
.gridItem.expanded:hover {
  background-color: #fff;
  color: #2a2a2a;
}
