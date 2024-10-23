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
      contextual: 'In the digital age, learning has become a continuous process that is easily accessible and adaptable to personal needs.',
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



.mainContent {
  flex-grow: 1;
  padding: 35px;
  padding-top: 60px;
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
  color: #333;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.chevronIcon {
  font-size: 1em;
  color: #888;
  transition: transform 0.3s ease;
}

/* Grid styling for the first question */
.gridAnswer {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.gridItem {
  padding: 20px;
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease;
}

.gridItem:hover {
  transform: translateY(-5px);
}

.gridItem h3 {
  font-size: 1.2em;
  margin-bottom: 10px;
  color: #333;
}

.gridItem p {
  font-size: 1em;
  color: #555;
  line-height: 1.6;
}

.answer {
  padding: 15px;
  background-color: #f8f9fa;
  font-size: 1em;
  line-height: 1.6;
  color: #555;
}
