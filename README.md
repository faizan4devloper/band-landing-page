import React, { useState } from 'react';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';

const contentData = [
  {
    question: 'What is the best way to start learning new skills?',
    answer: 'Start with identifying your interests, then find online courses, books, or mentors in that field.',
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
    // When the same question is clicked again, collapse it, otherwise open the new one and hide others
    setOpenQuestion(openQuestion === index ? null : index);
  };

  return (
    <div className={styles.mainContent}>
      {contentData.map((item, index) => (
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
          {/* Only the answer of the open question is shown */}
          {openQuestion === index && (
            <div className={styles.answer}>
              {item.answer}
            </div>
          )}
        </div>
      ))}
    </div>
  );
};

export default MainContent;
