import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import PropagateLoader from 'react-spinners/PropagateLoader';
import { faChevronDown, faChevronUp, faCaretDown, faCaretUp } from '@fortawesome/free-solid-svg-icons';

const MainContent = ({ activeTopic }) => {
  const [contentData, setContentData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [openQuestion, setOpenQuestion] = useState(null);
  const [openGridItems, setOpenGridItems] = useState({
    textualResponse: true,
  });

  const topicQuestions = {
    1: [
      'What are the average class sizes and student-teacher ratios in the local schools?',
      'What are the admission criteria for the schools in this area?',
    ],
    // Add other topic questions here...
  };

  const fetchDataForQuestion = async (question) => {
    try {
      const response = await axios.post('dummy', { question: `${question}:- hi` });
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
    setLoading(true);
    try {
      const questionsList = topicQuestions[topicId] || [];
      const formattedData = await Promise.all(
        questionsList.map(async (question) => {
          const answer = await fetchDataForQuestion(question);
          return { question, answer: { textualResponse: answer } };
        })
      );
      setContentData(formattedData);
      setLoading(false);
    } catch (error) {
      console.error('Error fetching data:', error);
      setLoading(false);
    }
  };

  useEffect(() => {
    fetchAllData(activeTopic);
  }, [activeTopic]);

  const toggleAnswer = (index) => {
    setOpenQuestion(prevIndex => (prevIndex === index ? null : index));
  };

  return (
    <div className={styles.mainContent}>
      {loading ? (
        <div className={styles.loaderWrapper}>
          <PropagateLoader color="rgb(15, 95, 220)" loading={loading} size={22} />
        </div>
      ) : (
        contentData.map((item, index) => (
          <div
            key={index}
            className={`${styles.questionBlock} ${index % 2 === 0 ? styles.even : styles.odd}`}
          >
            <div className={styles.question} onClick={() => toggleAnswer(index)}>
              <FontAwesomeIcon icon={faChevronDown} />
              <span>{item.question}</span>
              <FontAwesomeIcon icon={openQuestion === index ? faChevronUp : faChevronDown} />
            </div>
            {openQuestion === index && (
              <div className={styles.answerContainer}>
                <p>{item.answer.textualResponse.join(', ')}</p>
                {/* Add action buttons here */}
              </div>
            )}
          </div>
        ))
      )}
    </div>
  );
};

export default MainContent;



.mainContent {
  padding: 30px;
  background-color: #f7f9fc;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  width: 800px;
}

.questionBlock {
  border-radius: 10px;
  margin-bottom: 15px;
  transition: background-color 0.3s ease, box-shadow 0.3s ease;
}

.questionBlock:hover {
  background-color: #f1f4f9;
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.1);
}

.even {
  background-color: #ffffff;
}

.odd {
  background-color: #f8fafc;
}

.question {
  font-size: 1rem;
  font-weight: bold;
  padding: 14px;
  cursor: pointer;
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: #2a2a2a;
}

.answerContainer {
  padding: 10px;
  background-color: #eff0f7;
  border-radius: 8px;
  margin-top: 10px;
  color: #555;
}

.loaderWrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 60vh;
}
