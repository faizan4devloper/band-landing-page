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

 
  
const fetchDataForQuestion = async (question) => {
  const payload = {
    question_id: question.id,
    question: question.text,
  };

  try {
    const response = await axios.post(
      'https://2kn1kfoouh.execute-api.us-east-1.amazonaws.com/edu/cit-adv2', // Replace with your actual API endpoint
      payload,
      { headers: { 'Content-Type': 'application/json' } }
    );

    console.log('API Response:', response.data)

    // Check if `response.data.body` is already an object or a JSON string
    const parsedResponse = typeof response.data.body === 'string'
      ? JSON.parse(response.data.body)
      : response.data.body;

    console.log(parsedResponse)

    const llmAnswer = parsedResponse.answer || '';
    const formattedAnswer = llmAnswer.split('-').map(line => line.trim()).filter(line => line);

    const factualData = parsedResponse.factualData && parsedResponse.factualData.startsWith("http")
      ? parsedResponse.factualData
      : 'No factual information available.';
      
    const citizenReview = parsedResponse.citizenReview && parsedResponse.citizenReview.startsWith("http")
      ? parsedResponse.citizenReview
      : 'No citizen experience information available.';

    const Source = parsedResponse.Source || 'No source available.'; // Add Source field

    return {
      question: question.text,
      answer: {
        textualResponse: formattedAnswer.length > 0 ? [...formattedAnswer, `Source: ${Source}`] : ['No Answer Available'],
        factualData: factualData,
        citizenReview: citizenReview,
        contextual: parsedResponse.contextual || 'No contextual information available.',
      },
    };
  } catch (error) {
    console.error("Error fetching data for question:", error);
    return {
      question: question.text,
      answer: {
        textualResponse: ['No Answer Available'],
        factualData: 'No factual information available.',
        citizenReview: 'No citizen experience information available.',
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



.mainContent {
  flex-grow: 1;
  height: 100%;
  padding:0px 35px;
  /*padding-bottom: 160px;*/
  /*background-color: #f7f9fc;*/
  overflow-y: auto;
  width: 800px;
  /*border-radius: 10px;*/
  overflow-y: auto;
  /*box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);*/
  display: flex;
  flex-direction: column;
}

.loaderWrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 60vh;
}

.selectedQuestionBlock {
  padding:0px 25px 0px 0px;
  margin-top: 20px; 
  border: 1px solid  rgba(95, 30, 193, 0.1); /* Light purple on hover */
  border-radius: 5px;
  background-color: #f8f9fa; /* Light background for selected question */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Added shadow for depth */
}


.mainContent::-webkit-scrollbar{
  /*width: 8px;*/
  display:none;
}

/*.mainContent::-webkit-scrollbar-track {*/
/*  background: #f1f1f1;*/
/*}*/

/*.mainContent::-webkit-scrollbar-thumb {*/
/*  background-color: #888;*/
/*  border-radius: 8px;*/
/*  border: 2px solid #f1f1f1;*/
/*}*/

/*.mainContent::-webkit-scrollbar-thumb:hover {*/
/*  background-color: #555;*/
/*}*/
