import React, { useState, useEffect } from 'react';
import axios from 'axios';
import PropagateLoader from 'react-spinners/PropagateLoader';
import FaqDropdown from './FaqDropdown';
import QuestionBlock from './QuestionBlock';
import styles from './MainContent.module.css';

const MainContent = ({ activeTopic, onQuestionClick }) => {
  const [contentData, setContentData] = useState([]); // Stores fetched data for questions
  const [loading, setLoading] = useState(true); // Loader state
  const [selectedData, setSelectedData] = useState({}); // Stores selected question data

  const topicQuestions = {
    1: [
      { id: 'q1', text: 'What are the last five years key statistics for Serpell Primary School?' },
      { id: 'q2', text: 'What are the admission criteria and process for Serpell Primary School?' },
      { id: 'q3', text: 'How does the school perform in standardized tests and assessments?' },
    ],
    // Other topics...
  };

  const fetchDataForQuestion = async ({ id, text }) => {
    try {
      const response = await axios.post(
        'dummy', // Replace with your actual endpoint
        { questionId: id, questionText: text },
        { headers: { 'Content-Type': 'application/json' } }
      );
      const parsedResponse = JSON.parse(response.data.body);
      const llmAnswer = parsedResponse.answer || '';

      return {
        textualResponse: llmAnswer.split('-').map(line => line.trim()).filter(line => line),
        factualData: parsedResponse.factualData,
        citizenReview: parsedResponse.citizenReview,
        contextual: parsedResponse.contextual || 'No contextual information available.',
      };
    } catch (error) {
      return { textualResponse: ['No Answer Available'] };
    }
  };

  const fetchAllData = async (topicId) => {
    setLoading(true);
    try {
      const questionsList = topicQuestions[topicId] || [];
      const formattedData = await Promise.all(
        questionsList.map(async (question) => {
          const answerData = await fetchDataForQuestion(question);
          return { question: question.text, answer: answerData };
        })
      );

      setContentData(formattedData);
      setLoading(false);
    } catch (error) {
      setLoading(false);
    }
  };

  const handleQuestionSelect = (question, answer) => {
    setSelectedData({ question, answer });
    onQuestionClick(question, answer); // Pass question to chatbot on click
  };

  useEffect(() => {
    fetchAllData(activeTopic);
    setSelectedData({});
  }, [activeTopic]);

  return (
    <div className={styles.mainContent}>
      {loading ? (
        <div className={styles.loaderWrapper}>
          <PropagateLoader color="rgb(15, 95, 220)" loading={loading} size={22} />
        </div>
      ) : (
        <>
          <FaqDropdown contentData={contentData} onQuestionSelect={handleQuestionSelect} />
          {selectedData.question && (
            <QuestionBlock question={selectedData.question} answerData={selectedData.answer} />
          )}
        </>
      )}
    </div>
  );
};

export default MainContent;





import React, { useState } from 'react';
import Sidebar from './Sidebar';
import MainContent from './MainContent';
import Chatbot from './Chatbot';
import styles from './EducationPage.module.css';

const EducationPage = () => {
  const [activeTopic, setActiveTopic] = useState(1);
  const [selectedQuestion, setSelectedQuestion] = useState(null);

  const handleQuestionClick = (question, answer) => {
    setSelectedQuestion({ question, answer });
  };

  return (
    <div className={styles.educationPage}>
      <Sidebar setActiveTopic={setActiveTopic} />
      <MainContent activeTopic={activeTopic} onQuestionClick={handleQuestionClick} />
      <Chatbot questionFromMain={selectedQuestion} />
    </div>
  );
};

export default EducationPage;
