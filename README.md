import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import PropagateLoader from 'react-spinners/PropagateLoader';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';

const MainContent = ({ activeTopic }) => {
  const [contentData, setContentData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [openQuestion, setOpenQuestion] = useState(null); // Initially no question expanded
  const [openFAQ, setOpenFAQ] = useState(false); // FAQ dropdown control

  const topicQuestions = {
    1: [
      'What are the average class sizes and student-teacher ratios in the local schools?',
      'What are the admission criteria for the schools in this area?',
    ],
    // Other topics...
  };

  const fetchDataForQuestion = async (question) => {
    try {
      const response = await axios.post(
        'dummy', 
        { question: `${question}:- hi` },
        { headers: { 'Content-Type': 'application/json' } }
      );

      const parsedResponse = JSON.parse(response.data.body);
      const llmAnswer = parsedResponse.answer;
      return llmAnswer ? llmAnswer.split('-').map(line => line.trim()) : ['No Answer Available'];
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
          return { question, answer };
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

  const toggleFAQDropdown = () => {
    setOpenFAQ((prevState) => !prevState);
  };

  const toggleAnswer = (index) => {
    setOpenQuestion(prevIndex => prevIndex === index ? null : index);
  };

  return (
    <div className={styles.mainContent}>
      <div className={styles.faqDropdown} onClick={toggleFAQDropdown}>
        <h2>FAQ</h2>
        <FontAwesomeIcon icon={openFAQ ? faChevronUp : faChevronDown} />
      </div>

      {openFAQ && (
        <div className={styles.dropdownContent}>
          {loading ? (
            <div className={styles.loaderWrapper}>
              <PropagateLoader color="rgb(15, 95, 220)" loading={loading} size={22} />
            </div>
          ) : contentData.map((item, index) => (
            <div key={index} className={styles.questionBlock}>
              <div className={styles.question} onClick={() => toggleAnswer(index)}>
                {item.question}
                <FontAwesomeIcon
                  icon={openQuestion === index ? faChevronUp : faChevronDown}
                  className={styles.chevronIcon}
                />
              </div>
              {openQuestion === index && (
                <div className={styles.answer}>
                  {item.answer.map((line, idx) => (
                    <p key={idx}>{line}</p>
                  ))}
                </div>
              )}
            </div>
          ))}
        </div>
      )}
    </div>
  );
};

export default MainContent;




.faqDropdown {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px;
  cursor: pointer;
  font-size: 1.2em;
  font-weight: bold;
  background-color: #f7f9fc;
  border-radius: 8px;
  transition: background-color 0.3s ease;
}

.faqDropdown:hover {
  background-color: #eef2f7;
}

.dropdownContent {
  padding-top: 15px;
  padding-bottom: 15px;
  border-top: 1px solid #ddd;
}

.answer {
  padding: 10px 15px;
  background-color: #ffffff;
  border-radius: 5px;
  font-size: 0.9em;
  color: #555;
  transition: max-height 0.3s ease;
}

