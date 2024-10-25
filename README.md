import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';
import PropagateLoader from 'react-spinners/PropagateLoader';
import QuestionBlock from './QuestionBlock';

const MainContent = ({ activeTopic }) => {
  const [contentData, setContentData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [openQuestion, setOpenQuestion] = useState(null); // Initially no question expanded

  const topicQuestions = {
    1: [
      'What are the average class sizes and student-teacher ratios in the local schools?',
      'What are the admission criteria for the schools in this area?',
    ],
    2: ['Topic 2 Question 1', 'Topic 2 Question 2'],
    // Add more topics as needed...
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
      const formattedAnswer = llmAnswer.split('-').map(line => line.trim()).filter(line => line);

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
    fetchAllData(activeTopic);
  }, [activeTopic]);

  const toggleAnswer = (index) => setOpenQuestion(prevIndex => (prevIndex === index ? null : index));

  return (
    <div className={styles.mainContent}>
      {loading ? (
        <div className={styles.loaderWrapper}>
          <PropagateLoader color="rgb(15, 95, 220)" loading={loading} size={22} />
        </div>
      ) : contentData.length > 0 ? (
        contentData.map((item, index) => (
          <QuestionBlock
            key={index}
            question={item.question}
            answerData={item.answer}
            isOpen={openQuestion === index}
            toggle={() => toggleAnswer(index)}
          />
        ))
      ) : (
        <div>No Questions Available</div>
      )}
    </div>
  );
};

export default MainContent;




import React from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';
import styles from './QuestionBlock.module.css';
import GridAnswer from './GridAnswer';

const QuestionBlock = ({ question, answerData, isOpen, toggle }) => (
  <div className={styles.questionBlock}>
    <div className={styles.question} onClick={toggle}>
      {question}
      <FontAwesomeIcon icon={isOpen ? faChevronUp : faChevronDown} className={styles.chevronIcon} />
    </div>
    {isOpen && <GridAnswer answerData={answerData} />}
  </div>
);

export default QuestionBlock;








import React, { useState } from 'react';
import styles from './GridAnswer.module.css';
import GridItem from './GridItem';

const GridAnswer = ({ answerData }) => {
  const [openGridItems, setOpenGridItems] = useState({
    textualResponse: true,
  });

  const toggleGridItem = (section) => {
    setOpenGridItems(prevState => ({
      ...prevState,
      [section]: !prevState[section],
    }));
  };

  return (
    <div className={styles.gridAnswer}>
      <GridItem
        title="Textual Response"
        isOpen={openGridItems.textualResponse}
        toggle={() => toggleGridItem('textualResponse')}
        content={answerData.textualResponse}
      />
      <GridItem
        title="Citizen Experience"
        isOpen={openGridItems.citizenExperience}
        toggle={() => toggleGridItem('citizenExperience')}
        content={[answerData.citizenExperience]}
      />
      <GridItem
        title="Factual Info"
        isOpen={openGridItems.factualInfo}
        toggle={() => toggleGridItem('factualInfo')}
        content={[answerData.factualInfo]}
      />
      <GridItem
        title="Contextual"
        isOpen={openGridItems.contextual}
        toggle={() => toggleGridItem('contextual')}
        content={[answerData.contextual]}
      />
    </div>
  );
};

export default GridAnswer;




import React from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faCaretDown, faCaretUp } from '@fortawesome/free-solid-svg-icons';
import styles from './GridItem.module.css';

const GridItem = ({ title, isOpen, toggle, content }) => (
  <div className={`${styles.gridItem} ${isOpen ? styles.expanded : styles.collapsed}`} onClick={toggle}>
    <h3>{title}</h3>
    <FontAwesomeIcon icon={isOpen ? faCaretUp : faCaretDown} className={styles.chevronIcon} />
    {isOpen && (
      <ul className={styles.answerList}>
        {content.map((line, index) => (
          <li key={index}>{line}</li>
        ))}
      </ul>
    )}
  </div>
);

export default GridItem;
