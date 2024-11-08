import React, { useState, useEffect } from 'react';
import axios from 'axios';
import PropagateLoader from 'react-spinners/PropagateLoader';
import FaqDropdown from './FaqDropdown';
import QuestionBlock from './QuestionBlock';
import styles from './MainContent.module.css';

const MainContent = ({ activeTopic }) => {
  const [topics, setTopics] = useState([]); // Dynamically fetched topics
  const [contentData, setContentData] = useState([]); // Answers for questions in the selected topic
  const [loading, setLoading] = useState(true);
  const [selectedData, setSelectedData] = useState({});
  const [questions, setQuestions] = useState([]); // Dynamically loaded questions

  // Fetch topics on component load
  const fetchTopics = async () => {
    try {
      const response = await axios.get('your-topics-endpoint'); // Replace with your endpoint for topics
      setTopics(response.data); // Assuming response data is an array of topic objects
    } catch (error) {
      console.error("Error fetching topics:", error);
    }
  };

  // Fetch questions for a specific topic
  const fetchQuestionsForTopic = async (topicId) => {
    try {
      setLoading(true);
      const response = await axios.get(`your-questions-endpoint/${topicId}`); // Replace with your endpoint
      setQuestions(response.data); // Assuming response data is an array of questions
      setLoading(false);
    } catch (error) {
      console.error("Error fetching questions:", error);
      setLoading(false);
    }
  };

  // Fetch data dynamically for each question using the provided payload structure
  const fetchDataForQuestion = async (question) => {
    const payload = {
      question_id: question.id,
      question: question.text,
    };

    try {
      const response = await axios.post(
        'your-answers-endpoint', // Replace with your actual endpoint for answers
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
        textualResponse: formattedAnswer.length > 0 ? formattedAnswer : ['No Answer Available'],
        factualData: factualData || 'No factual information available.',
        citizenReview: citizenReview || 'No citizen experience information available.',
        contextual: parsedResponse.contextual || 'No contextual information available.',
      };
    } catch (error) {
      console.error("Error fetching data for question:", error);
      return {
        textualResponse: ['No Answer Available'],
        factualData: null,
        citizenReview: null,
        contextual: 'No contextual information available.',
      };
    }
  };

  const fetchAllData = async (topicId) => {
    setLoading(true);
    try {
      const formattedData = await Promise.all(
        questions.map(async (question) => {
          const answerData = await fetchDataForQuestion(question);
          return {
            question: question.text,
            answer: answerData,
          };
        })
      );

      setContentData(formattedData);
      setLoading(false);
    } catch (error) {
      console.error("Error fetching all data:", error);
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

  // Load topics on initial render
  useEffect(() => {
    fetchTopics();
  }, []);

  // Load questions when active topic changes
  useEffect(() => {
    if (activeTopic) {
      fetchQuestionsForTopic(activeTopic);
      setSelectedData((prev) => ({ ...prev, [activeTopic]: null }));
    }
  }, [activeTopic]);

  // Load answers once questions are fetched
  useEffect(() => {
    if (questions.length > 0) {
      fetchAllData(activeTopic);
    }
  }, [questions]);

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
