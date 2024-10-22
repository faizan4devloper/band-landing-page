import React, { useState } from 'react';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons'; // Import FontAwesome chevron icons

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



.mainContent {
  flex-grow: 1;
  padding: 35px;
  background-color: #f7f9fc;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.questionBlock {
  margin-bottom: 15px;
  border: 1px solid #ddd;
  border-radius: 8px;
  background-color: #fff;
  transition: background-color 0.3s ease;
}

.questionBlock:hover {
  background-color: #f1f1f1;
}

.question {
  font-size: 1.1em;
  padding: 10px;
  cursor: pointer;
  /*font-weight: bold;*/
  color: #333;
  display: flex;
  justify-content: space-between; /* This will move the chevron to the right */
  align-items: center; /* This centers the text and icon vertically */
}

.chevronIcon {
  font-size: 1em;
  color: #888;
  transition: transform 0.3s ease;
}

.answer {
  padding: 15px;
  background-color: #f8f9fa;
  font-size: 1em;
  line-height: 1.6;
  color: #555;
}

.questionBlock .answer {
  animation: slideDown 0.4s ease-out;
}

@keyframes slideDown {
  0% {
    max-height: 0;
    opacity: 0;
  }
  100% {
    max-height: 500px;
    opacity: 1;
  }
}
