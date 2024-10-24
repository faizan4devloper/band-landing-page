import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import PropagateLoader from 'react-spinners/PropagateLoader';
import { faChevronDown, faChevronUp, faCaretDown, faCaretUp } from '@fortawesome/free-solid-svg-icons';

const MainContent = () => {
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
        'dummy', // Replace 'dummy' with your actual API endpoint
        {
          question: 'What are the average class sizes and student-teacher ratios in the local schools?:- hi',
        },
        {
          headers: {
            'Content-Type': 'application/json',
          },
        }
      );

      const parsedResponse = JSON.parse(response.data.body);
      const llmAnswer = parsedResponse.answer;

      // Split answer into lines based on '-' hyphen
      const formattedAnswer = llmAnswer.split('-').map((line) => line.trim()).filter(line => line);

      const formattedData = [
        {
          question: 'What are the average class sizes and student-teacher ratios in the local schools?',
          answer: {
            textualResponse: formattedAnswer.length > 0 ? formattedAnswer : ['No Answer Available'],
            citizenExperience: 'Citizen experience response goes here.',
            factualInfo: 'Factual information goes here.',
            contextual: 'Contextual information goes here.',
          },
        },
        {
          question: 'What are the admission criteria for the schools in this area? How do they prioritize applications?',
          answer: {
            textualResponse: formattedAnswer.length > 0 ? formattedAnswer : ['No Answer Available'],
            citizenExperience: 'Citizen experience response goes here.',
            factualInfo: 'Factual information goes here.',
            contextual: 'Contextual information goes here.',
          },
        },
        {
          question: 'What extracurricular activities are offered in the local schools?',
          answer: {
            textualResponse: ['Extracurricular activities include sports, music, art, and drama.'],
            citizenExperience: 'The schools provide ample opportunities for student involvement in extracurriculars.',
            factualInfo: 'Most schools offer after-school programs, including competitive sports teams and arts clubs.',
            contextual: 'Participation in extracurricular activities is encouraged as part of the holistic development approach.',
          },
        },
        {
          question: 'How do local schools support students with special needs or learning disabilities?',
          answer: {
            textualResponse: ['Schools have specialized support programs for students with learning disabilities.'],
            citizenExperience: 'Parents have reported positive experiences with the special education programs.',
            factualInfo: 'Schools offer individualized education programs (IEPs) and provide access to trained professionals.',
            contextual: 'The inclusion programs are well-funded, ensuring adequate resources for students with special needs.',
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
    fetchData();
  }, []);

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
        ))
      ) : (
        <div>No data available</div>
      )}
    </div>
  );
};

export default MainContent;
