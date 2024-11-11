MainContent.js:66 Error fetching data: SyntaxError: "undefined" is not valid JSON
    at JSON.parse (<anonymous>)
    at fetchDataForQuestion (MainContent.js:41:1)
    at async MainContent.js:82:1
    at async Promise.all (index 0)
    at async fetchAllData (MainContent.js:80:1)





    import React, { useState, useEffect } from 'react';
import axios from 'axios';
import PropagateLoader from 'react-spinners/PropagateLoader';
import FaqDropdown from './FaqDropdown';
import QuestionBlock from './QuestionBlock';
import styles from './MainContent.module.css';

const MainContent = ({ activeTopic }) => {
  const [questions, setQuestions] = useState([]); // Dynamically loaded questions for the topic
  const [contentData, setContentData] = useState([]); // Stores answers for each question
  const [loading, setLoading] = useState(true);
  const [selectedData, setSelectedData] = useState({});

  // Define the questions based on the active topic
  const topicQuestions = {
    1: [
      { id: 'Q1', text: 'What are the last five years key statistics for Serpell Primary School?' },
      { id: 'Q2', text: 'What are the admission criteria and process for Serpell Primary School?' },
      { id: 'Q3', text: 'How does the school perform in standardized tests and assessments?' },
    ],
    2: [
      { id: 'Q4', text: 'What’s the curriculum at Serpell Primary School?' },
      { id: 'Q5', text: 'How is the curriculum structured across different year levels?' },
      { id: 'Q6', text: 'What specialist programs are offered at Serpell Primary School?' },
    ],
    3: [
      { id: 'Q7', text: 'How does the school engage with the broader community?' },
      { id: 'Q8', text: 'How can parents get involved in the school community?' },
      { id: 'Q9', text: 'What support services are available for students with special needs at Serpell Primary School?' },
    ],
  };

  // Fetch data for each question by calling a single POST API
  const fetchDataForQuestion = async (question) => {
    const payload = {
      question_id: question.id,
      question: question.text,
    };

    try {
      const response = await axios.post(
        'dummy', // Replace with your actual API endpoint
        payload,
        { headers: { 'Content-Type': 'application/json' } }
      );

      const parsedResponse = JSON.parse(response.data.body);
      const llmAnswer = parsedResponse.answer || '';
      const formattedAnswer = llmAnswer.split('-').map(line => line.trim()).filter(line => line);

      const factualData = parsedResponse.factualData && parsedResponse.factualData.startsWith("http")
        ? parsedResponse.factualData
        : null;

      const citizenReview = parsedResponse.citizenReview && parsedResponse.citizenReview.startsWith("http")
        ? parsedResponse.citizenReview
        : null;

      return {
        question: question.text,
        answer: {
          textualResponse: formattedAnswer.length > 0 ? formattedAnswer : ['No Answer Available'],
          factualData: factualData || 'No factual information available.',
          citizenReview: citizenReview || 'No citizen experience information available.',
          contextual: parsedResponse.contextual || 'No contextual information available.',
        },
      };
    } catch (error) {
      console.error("Error fetching data for question:", error);
      return {
        question: question.text,
        answer: {
          textualResponse: ['No Answer Available'],
          factualData: null,
          citizenReview: null,
          contextual: 'No contextual information available.',
        },
      };
    }
  };

  // Fetch all data for the selected topic
  const fetchAllData = async (topicId) => {
    setLoading(true);
    try {
      const questionsList = topicQuestions[topicId] || [];
      const formattedData = await Promise.all(
        questionsList.map(fetchDataForQuestion)
      );
      setContentData(formattedData);
    } catch (error) {
      console.error("Error fetching data for all questions:", error);
    } finally {
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
