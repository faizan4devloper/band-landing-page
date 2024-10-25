import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';
import Loader from './Loader';
import QuestionBlock from './QuestionBlock';

const MainContent = ({ activeTopic }) => {
  const [contentData, setContentData] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    setLoading(true);
    fetchAllData(activeTopic);
  }, [activeTopic]);

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

  return (
    <div className={styles.mainContent}>
      {loading ? (
        <Loader />
      ) : contentData.length > 0 ? (
        contentData.map((item, index) => (
          <QuestionBlock key={index} question={item.question} answer={item.answer} />
        ))
      ) : (
        <div>No Questions Available</div>
      )}
    </div>
  );
};

export default MainContent;




import React, { useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';
import GridItem from './GridItem';
import styles from './QuestionBlock.module.css';

const QuestionBlock = ({ question, answer }) => {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <div className={styles.questionBlock}>
      <div className={styles.question} onClick={() => setIsOpen(!isOpen)}>
        {question}
        <FontAwesomeIcon icon={isOpen ? faChevronUp : faChevronDown} className={styles.chevronIcon} />
      </div>
      {isOpen && (
        <div className={styles.gridAnswer}>
          <GridItem title="Textual Response" content={answer.textualResponse} />
          <GridItem title="Citizen Experience" content={answer.citizenExperience} />
          <GridItem title="Factual Info" content={answer.factualInfo} />
          <GridItem title="Contextual" content={answer.contextual} />
        </div>
      )}
    </div>
  );
};

export default QuestionBlock;





import React, { useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faCaretDown, faCaretUp } from '@fortawesome/free-solid-svg-icons';
import styles from './GridItem.module.css';

const GridItem = ({ title, content }) => {
  const [isExpanded, setIsExpanded] = useState(true);

  return (
    <div className={`${styles.gridItem} ${isExpanded ? styles.expanded : styles.collapsed}`} onClick={() => setIsExpanded(!isExpanded)}>
      <h3>{title}</h3>
      <FontAwesomeIcon icon={isExpanded ? faCaretUp : faCaretDown} className={styles.chevronIcon} />
      {isExpanded && (
        <ul className={styles.answerList}>
          {Array.isArray(content) ? content.map((line, idx) => <li key={idx}>- {line}</li>) : <li>{content}</li>}
        </ul>
      )}
    </div>
  );
};

export default GridItem;




import React from 'react';
import PropagateLoader from 'react-spinners/PropagateLoader';
import styles from './Loader.module.css';

const Loader = () => (
  <div className={styles.loaderWrapper}>
    <PropagateLoader color="rgb(15, 95, 220)" size={22} />
  </div>
);

export default Loader;
