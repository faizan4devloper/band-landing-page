import React, { useState, useEffect } from 'react';
import axios from 'axios';
import PropagateLoader from 'react-spinners/PropagateLoader';
import FaqDropdown from './FaqDropdown';
import QuestionBlock from './QuestionBlock';
import styles from './MainContent.module.css';

const MainContent = ({ activeTopic }) => {
  const [contentData, setContentData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [selectedData, setSelectedData] = useState({});

  const topicQuestions = {
    1: [
      'What are the last five years key statistics for Serpell Primary School?',
      'What are the admission criteria and process for Serpell Primary School?',
      'How does the school perform in standardized tests and assessments?'
    ],
    2: [
      'What’s the curriculum at Serpell Primary School?',
      'How is the curriculum structured across different year levels?',
      'What specialist programs are offered at Serpell Primary School?'
    ],
    3: [
      'How does the school engage with the broader community?',
      'How can parents get involved in the school community?',
      'What support services are available for students with special needs at Serpell Primary School?'
    ],
    // Add more topics as needed...
  };

  const fetchDataForQuestion = async (question) => {
    try {
      const response = await axios.post(
        'dummy',
        { question: `${question}` },
        { headers: { 'Content-Type': 'application/json' } }
      );    

      // Log the raw response data
      console.log('Raw response data for question:', question, response.data);

      const parsedResponse = JSON.parse(response.data.body);
      const llmAnswer = parsedResponse.answer;

      // Log the parsed answer
      console.log('Parsed answer:', llmAnswer);

      const formattedAnswer = llmAnswer.split('-').map(line => line.trim()).filter(line => line);

      // Log the formatted answer
      console.log('Formatted answer:', formattedAnswer);

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
              citizenExperience: 'Citizen experience response goes here. its me kkkkkkkslf dfksfjdlf sdkfjslf jkdlfaas lksdddddddf ksdfjlafkls ksaldfjlsfj sdfupwie3 fdskfjlkasf 3 rwkslfjlsad',
              factualInfo: 'Factual information goes here.',
              contextual: 'Contextual information goes here.',
            },
          };
        })
      );

      // Log the formatted content data
      console.log('Formatted content data:', formattedData);

      setContentData(formattedData);
      setLoading(false);
    } catch (error) {
      console.error('Error fetching data for all questions:', error);
      setLoading(false);
    }
  };

  const handleQuestionSelect = (question, answer) => {
    setSelectedData((prev) => ({
      ...prev,
      [activeTopic]: {
        question,
        answer,
      },
    }));
  };

  useEffect(() => {
    fetchAllData(activeTopic);
    setSelectedData((prev) => ({ ...prev, [activeTopic]: null }));
  }, [activeTopic]);

  const selectedQuestionData = selectedData[activeTopic] || {};

  return (
    <div className={styles.mainContent}>
      {loading ? (
        <div className={styles.loaderWrapper}>
          <PropagateLoader color="rgb(15, 95, 220)" loading={loading} size={22} />
        </div>
      ) : (
        <>
          <FaqDropdown
            contentData={contentData}
            onQuestionSelect={handleQuestionSelect}
            selectedQuestion={selectedQuestionData.question}
            selectedAnswer={selectedQuestionData.answer}
          />
          {selectedQuestionData.question && selectedQuestionData.answer && (
            <div className={styles.selectedQuestionBlock}>
              <QuestionBlock
                question={selectedQuestionData.question}
                answerData={selectedQuestionData.answer}
              />
            </div>
          )}
        </>
      )}
    </div>
  );
};

export default MainContent;








import React, { useState, useEffect } from 'react';
import axios from 'axios';
import PropagateLoader from 'react-spinners/PropagateLoader';
import FaqDropdown from './FaqDropdown';
import QuestionBlock from './QuestionBlock';
import styles from './MainContent.module.css';

const MainContent = ({ activeTopic }) => {
  const [contentData, setContentData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [selectedData, setSelectedData] = useState({}); // Use an object to store selected question and answer by topic

  const topicQuestions = {
    1: [
      'What are the last five years key statistics for Serpell Primary School?',
      'What are the admission criteria and process for Serpell Primary School?',
      'How does the school perform in standardized tests and assessments?'
    ],
    2: [
    'What’s the curriculum at Serpell Primary School?',
    'How is the curriculum structured across different year levels?',
    'What specialist programs are offered at Serpell Primary School?'
    ],
    3: [
      'How does the school engage with the broader community?',
      'How can parents get involved in the school community?',
      'What support services are available for students with special needs at serpell primary school?'
      ],
    // Add more topics as needed...
  };

  const fetchDataForQuestion = async (question) => {
    try {
      const response = await axios.post(
        'dummy',
        { question: `${question}` },
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
              citizenExperience: 'Citizen experience response goes here. its me kkkkkkkslf dfksfjdlf sdkfjslf jkdlfaas lksdddddddf ksdfjlafkls ksaldfjlsfj sdfupwie3 fdskfjlkasf 3 rwkslfjlsad',
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

  const handleQuestionSelect = (question, answer) => {
    // Update selected data for the active topic
    setSelectedData((prev) => ({
      ...prev,
      [activeTopic]: {
        question,
        answer,
      },
    }));
  };

  useEffect(() => {
    fetchAllData(activeTopic);
    // Reset selected data when the active topic changes
    setSelectedData((prev) => ({ ...prev, [activeTopic]: null }));
  }, [activeTopic]);

  const selectedQuestionData = selectedData[activeTopic] || {};

  return (
    <div className={styles.mainContent}>
      {loading ? (
        <div className={styles.loaderWrapper}>
          <PropagateLoader color="rgb(15, 95, 220)" loading={loading} size={22} />
        </div>
      ) : (
        <>
          <FaqDropdown
            contentData={contentData}
            onQuestionSelect={handleQuestionSelect}
            selectedQuestion={selectedQuestionData.question}
            selectedAnswer={selectedQuestionData.answer}
          />
          {selectedQuestionData.question && selectedQuestionData.answer && (
            <div className={styles.selectedQuestionBlock}>
              <QuestionBlock
                question={selectedQuestionData.question}
                answerData={selectedQuestionData.answer}
              />
            </div>
          )}
        </>
      )}
    </div>
  );
};

export default MainContent;
