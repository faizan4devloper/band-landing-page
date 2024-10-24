import React, { useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronRight } from '@fortawesome/free-solid-svg-icons';
import styles from './Sidebar.module.css';

const topics = [
  { id: 1, name: 'Topic 1', question: 'What are the average class sizes and student-teacher ratios in the local schools?' },
  { id: 2, name: 'Topic 2', question: 'What are the admission criteria for the schools in this area? How do they prioritize applications?' },
  { id: 3, name: 'Topic 3', question: 'What extracurricular activities are available in the local schools?' },
  { id: 4, name: 'Topic 4', question: 'What are the key achievements of the schools in this area?' },
  { id: 5, name: 'Topic 5', question: 'How are the schools in this area addressing diversity and inclusion?' },
];

const Sidebar = ({ setActiveTopic }) => {
  const [active, setActive] = useState(1); // Default active topic

  const handleClick = (id, question) => {
    setActive(id);
    setActiveTopic(question); // Pass the question to the parent component
  };

  return (
    <div className={styles.sidebar}>
      <h2 className={styles.sidebarHeads}>Suggested Topics</h2>
      <ul>
        {topics.map((topic) => (
          <li
            key={topic.id}
            className={`${styles.topic} ${active === topic.id ? styles.active : ''}`}
            onClick={() => handleClick(topic.id, topic.question)}
          >
            <span className={styles.topicName}>{topic.name}</span>
            <FontAwesomeIcon icon={faChevronRight} className={styles.chevron} />
          </li>
        ))}
      </ul>
    </div>
  );
};

export default Sidebar;




import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import PropagateLoader from 'react-spinners/PropagateLoader';
import { faChevronDown, faChevronUp, faCaretDown, faCaretUp } from '@fortawesome/free-solid-svg-icons';

const MainContent = ({ activeQuestion }) => {
  const [contentData, setContentData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [openQuestion, setOpenQuestion] = useState(0);
  const [openGridItems, setOpenGridItems] = useState({
    textualResponse: true,
    citizenExperience: false,
    factualInfo: false,
    contextual: false,
  });

  const fetchData = async () => {
    try {
      const response = await axios.post(
        'dummy',  // Replace with your actual API
        {
          question: activeQuestion,  // Use the activeQuestion passed from Sidebar
        },
        {
          headers: {
            'Content-Type': 'application/json',
          },
        }
      );

      const parsedResponse = JSON.parse(response.data.body);
      const llmAnswer = parsedResponse.answer;

      const formattedAnswer = llmAnswer.split('-').map((line) => line.trim()).filter(line => line);

      const formattedData = [
        {
          question: activeQuestion,
          answer: {
            textualResponse: formattedAnswer.length > 0 ? formattedAnswer : ['No Answer Available'],
            citizenExperience: 'Citizen experience response goes here.',
            factualInfo: 'Factual information goes here.',
            contextual: 'Contextual information goes here.',
          },
        },
      ];

      setContentData(formattedData);
      setLoading(false);
    } catch (error) {
      console.error('Error fetching data:', error);
      setLoading(false);
    }
  };

  useEffect(() => {
    if (activeQuestion) {
      fetchData();
    }
  }, [activeQuestion]);

  const toggleAnswer = (index) => {
    if (openQuestion === index) {
      setOpenQuestion(null);
    } else {
      setOpenQuestion(index);
      setOpenGridItems({
        textualResponse: true,
        citizenExperience: false,
        factualInfo: false,
        contextual: false,
      });
    }
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
          <PropagateLoader color="rgb(15, 95, 220)" loading={loading} size={25} />
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

                {/* The other sections (citizenExperience, factualInfo, contextual) remain unchanged */}
              </div>
            )}
          </div>
        ))
      ) : (
        <div>No data available</div>
      )}
    </div>
  );
};

export default MainContent;

