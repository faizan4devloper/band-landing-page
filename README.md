API : https://dummy
payload : 
{
"question": "what are the averages?"
}

import React, { useState } from 'react';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';

const contentData = [
  {
    question: 'What is the best way to start learning new skills?',
    answer: {
      textual: 'Start with identifying your interests, then find online courses, books, or mentors in that field.',
      citizenExperience: 'Many citizens have shared success stories after using online platforms like Coursera, Udemy, and LinkedIn Learning.',
      factualInfo: 'According to research, individuals who continue learning new skills are 60% more likely to stay relevant in their jobs.',
      contextual: 'In the digital age, learning has become a continuous process that is easily accessible and adaptable to personal needs. ',
    }
  },
  {
    question: 'How can I find job opportunities?',
    answer: 'Use job search platforms, attend career fairs, and network with professionals in your field.',
  },
  {
    question: 'How can I manage my health records efficiently?',
    answer: 'You can use digital health apps or online portals provided by your healthcare provider.',
  },
  {
    question: 'What are the latest trends in technology?',
    answer: 'AI, blockchain, and quantum computing are currently revolutionizing the tech industry.',
  },
];

const MainContent = () => {
  const [openQuestion, setOpenQuestion] = useState(null);

  const toggleAnswer = (index) => {
    setOpenQuestion(openQuestion === index ? null : index);
  };

  return (
    <div className={styles.mainContent}>
      {openQuestion === null ? (
        contentData.map((item, index) => (
          <div key={index} className={styles.questionBlock}>
            <div
              className={styles.question}
              onClick={() => toggleAnswer(index)}
            >
              {item.question}
              <FontAwesomeIcon
                icon={faChevronDown}
                className={styles.chevronIcon}
              />
            </div>
          </div>
        ))
      ) : (
        <div className={styles.questionBlock}>
          <div
            className={styles.question}
            onClick={() => toggleAnswer(openQuestion)}
          >
            {contentData[openQuestion].question}
            <FontAwesomeIcon
              icon={faChevronUp}
              className={styles.chevronIcon}
            />
          </div>
          {/* Display grid layout for the first question */}
          {openQuestion === 0 ? (
            <div className={styles.gridAnswer}>
              <div className={styles.gridItem}>
                <h3>Textual Response</h3>
                <p>{contentData[0].answer.textual}</p>
              </div>
              <div className={styles.gridItem}>
                <h3>Citizen Experience</h3>
                <p>{contentData[0].answer.citizenExperience}</p>
              </div>
              <div className={styles.gridItem}>
                <h3>Factual Info</h3>
                <p>{contentData[0].answer.factualInfo}</p>
              </div>
              <div className={styles.gridItem}>
                <h3>Contextual</h3>
                <p>{contentData[0].answer.contextual}</p>
              </div>
            </div>
          ) : (
            <div className={styles.answer}>
              {contentData[openQuestion].answer}
            </div>
          )}
        </div>
      )}
    </div>
  );
};

export default MainContent;
