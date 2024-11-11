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
        'dummy', // Replace with your actual API endpoint
        payload,
        { headers: { 'Content-Type': 'application/json' } }
      );
      
      console.log('API Response:', response.data);

      // Check if `response.data.body` is already an object or a JSON string
      const parsedResponse = typeof response.data.body === 'string'
        ? JSON.parse(response.data.body)
        : response.data.body;

      const llmAnswer = parsedResponse.answer || '';
      const formattedAnswer = llmAnswer.split('-').map(line => line.trim()).filter(line => line);

      const factualData = parsedResponse.factualData && parsedResponse.factualData.startsWith("http")
        ? parsedResponse.factualData
        : 'No factual information available.';

      const citizenReview = parsedResponse.citizenReview && parsedResponse.citizenReview.startsWith("http")
        ? parsedResponse.citizenReview
        : 'No citizen experience information available.';

      return {
        question: question.text,
        answer: {
          textualResponse: formattedAnswer.length > 0 ? formattedAnswer : ['No Answer Available'],
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

  const handleGoClick = (question, answer) => {
    console.log("Go button clicked for:", question);
    // Add logic here to handle the "Go" button click, e.g., show the answer in a chat window or perform other actions
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
            onGoClick={handleGoClick} // Pass the onGoClick function as a prop
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







import React from 'react';
import styles from './FaqDropdown.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';

const FaqDropdown = ({ contentData, onQuestionSelect, selectedQuestion, onGoClick }) => {
  const [isFaqOpen, setIsFaqOpen] = React.useState(false);

  const toggleFaq = () => setIsFaqOpen(!isFaqOpen);

  const handleQuestionSelect = (question, answer) => {
    onQuestionSelect(question, answer);
    setIsFaqOpen(false);
  };

  const handleGoClick = (question, answer) => {
    if (onGoClick) {
      onGoClick(question, answer);  // Call the onGoClick function passed from parent
    }
  };

  return (
    <div className={styles.faqDropdown}>
      <div className={styles.dropdownHeader} onClick={toggleFaq}>
        <span>Frequently Asked Questions</span>
        <FontAwesomeIcon icon={isFaqOpen ? faChevronUp : faChevronDown} />
      </div>
      
      <div className={`${styles.dropdownContent} ${isFaqOpen ? styles.show : styles.hide}`}>
        {contentData.map((item, index) => (
          <div 
            key={index} 
            className={`${styles.questionBlock} ${selectedQuestion === item.question ? styles.activeQuestion : ''}`}
          >
            <span>{item.question}</span>
            {/* Go Button */}
            <button onClick={() => handleGoClick(item.question, item.answer)}>Go</button>
          </div>
        ))}
      </div>
    </div>
  );
};

export default FaqDropdown;
