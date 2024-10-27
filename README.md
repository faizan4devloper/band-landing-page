import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';
import PropagateLoader from 'react-spinners/PropagateLoader';
import QuestionBlock from './QuestionBlock';

const MainContent = ({ activeTopic }) => {
  const [contentData, setContentData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [selectedQuestionIndex, setSelectedQuestionIndex] = useState(null);
  const [faqData, setFaqData] = useState([]);
  const [openFaqIndex, setOpenFaqIndex] = useState(null); // State for managing opened FAQ

  const topicQuestions = {
    1: [
      'What are the average class sizes and student-teacher ratios in the local schools?',
      'What are the admission criteria for the schools in this area?',
    ],
    2: ['Topic 2 Question 1', 'Topic 2 Question 2'],
    // Add more topics as needed...
  };

  const faqs = [
    { question: "What is the purpose of this application?", answer: "This application helps users with inquiries regarding various topics." },
    { question: "How do I submit a question?", answer: "You can submit a question by selecting it from the available options." },
    // Add more FAQs as needed...
  ];

  const fetchDataForQuestion = async (question) => {
    try {
      const response = await axios.post(
        'dummy',
        { question: `${question}:- hi` },
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
              citizenExperience: 'Citizen experience response goes here.',
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

  useEffect(() => {
    fetchAllData(activeTopic);
  }, [activeTopic]);

  const selectQuestion = (index) => {
    setSelectedQuestionIndex(index);
  };

  const toggleFaq = (index) => {
    setOpenFaqIndex(openFaqIndex === index ? null : index); // Toggle FAQ
  };

  const selectedQuestion = selectedQuestionIndex !== null ? contentData[selectedQuestionIndex] : null;

  return (
    <div className={styles.mainContent}>
      {loading ? (
        <div className={styles.loaderWrapper}>
          <PropagateLoader color="rgb(15, 95, 220)" loading={loading} size={22} />
        </div>
      ) : contentData.length > 0 ? (
        <div>
          {contentData.map((item, index) => (
            <QuestionBlock
              key={index}
              question={item.question}
              answerData={item.answer}
              isOpen={selectedQuestionIndex === index}
              toggle={() => selectQuestion(index)}
            />
          ))}
          {selectedQuestion && (
            <div className={styles.selectedAnswer}>
              <h2>Selected Question:</h2>
              <h3>{selectedQuestion.question}</h3>
              <div>{selectedQuestion.answer.textualResponse.join(', ')}</div>
            </div>
          )}
          <h2>FAQs</h2>
          {faqs.map((faq, index) => (
            <QuestionBlock
              key={`faq-${index}`}
              question={faq.question}
              answerData={{ textualResponse: [faq.answer] }}
              isOpen={openFaqIndex === index}
              toggle={() => toggleFaq(index)} // Handle FAQ toggle
            />
          ))}
        </div>
      ) : (
        <div>No Questions Available</div>
      )}
    </div>
  );
};

export default MainContent;
