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
      { id: 'q1', text: 'What are the last five years key statistics for Serpell Primary School?' },
      { id: 'q2', text: 'What are the admission criteria and process for Serpell Primary School?' },
      { id: 'q3', text: 'How does the school perform in standardized tests and assessments?' }
    ],
    2: [
      { id: 'q4', text: 'What’s the curriculum at Serpell Primary School?' },
      { id: 'q5', text: 'How is the curriculum structured across different year levels?' },
      { id: 'q6', text: 'What specialist programs are offered at Serpell Primary School?' }
    ],
    3: [
      { id: 'q7', text: 'How does the school engage with the broader community?' },
      { id: 'q8', text: 'How can parents get involved in the school community?' },
      { id: 'q9', text: 'What support services are available for students with special needs at Serpell Primary School?' }
    ],
    // Add more topics as needed...
  };

  const fetchDataForQuestion = async ({ id, text }) => {
    try {
      console.log(`Fetching data for question ID: ${id}, Text: "${text}"`);
      const response = await axios.post(
        'dummy', // Replace with your actual endpoint
        { questionId: id, questionText: text },
        { headers: { 'Content-Type': 'application/json' } }
      );

      console.log('Raw response data:', response.data); // Log the raw response data
      const parsedResponse = JSON.parse(response.data.body);
      console.log('Parsed response:', parsedResponse); // Log the parsed response

      const llmAnswer = parsedResponse.answer;
      const formattedAnswer = llmAnswer.split('-').map(line => line.trim()).filter(line => line);
      console.log('Formatted answer:', formattedAnswer); // Log the formatted answer

      const factualInfo = parsedResponse.factualInfo?.startsWith('http') ? parsedResponse.factualInfo : null;
      const citizenExperience = parsedResponse.citizenExperience?.startsWith('http') ? parsedResponse.citizenExperience : null;

      return {
        textualResponse: formattedAnswer.length > 0 ? formattedAnswer : ['No Answer Available'],
        factualInfo: factualInfo || 'Factual information goes here.',
        citizenExperience: citizenExperience || 'Citizen experience response goes here.',
        contextual: parsedResponse.contextual || 'Contextual information goes here.',
      };
    } catch (error) {
      console.error('Error fetching data:', error);
      return {
        textualResponse: ['No Answer Available'],
        factualInfo: null,
        citizenExperience: null,
        contextual: 'No contextual information available.',
      };
    }
  };

  const fetchAllData = async (topicId) => {
    setLoading(true);
    try {
      const questionsList = topicQuestions[topicId] || [];
      console.log(`Fetching all data for topic ID: ${topicId}`, questionsList);

      const formattedData = await Promise.all(
        questionsList.map(async (question) => {
          const answerData = await fetchDataForQuestion(question);
          console.log(`Fetched data for question: "${question.text}"`, answerData); // Log each fetched answer data
          return {
            question: question.text,
            answer: answerData,
          };
        })
      );

      console.log('All fetched data for current topic:', formattedData); // Log all fetched data for the topic
      setContentData(formattedData);
      setLoading(false);
    } catch (error) {
      console.error('Error fetching data for all questions:', error);
      setLoading(false);
    }
  };

  const handleQuestionSelect = (question, answer) => {
    console.log('Selected question and answer:', question, answer); // Log selected question and answer
    setSelectedData((prev) => ({
      ...prev,
      [activeTopic]: {
        question,
        answer,
      },
    }));
  };

  useEffect(() => {
    console.log('Active topic changed:', activeTopic); // Log when active topic changes
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
  const [selectedData, setSelectedData] = useState({});

  const topicQuestions = {
    1: [
      { id: 'q1', text: 'What are the last five years key statistics for Serpell Primary School?' },
      { id: 'q2', text: 'What are the admission criteria and process for Serpell Primary School?' },
      { id: 'q3', text: 'How does the school perform in standardized tests and assessments?' }
    ],
    2: [
      { id: 'q4', text: 'What’s the curriculum at Serpell Primary School?' },
      { id: 'q5', text: 'How is the curriculum structured across different year levels?' },
      { id: 'q6', text: 'What specialist programs are offered at Serpell Primary School?' }
    ],
    3: [
      { id: 'q7', text: 'How does the school engage with the broader community?' },
      { id: 'q8', text: 'How can parents get involved in the school community?' },
      { id: 'q9', text: 'What support services are available for students with special needs at Serpell Primary School?' }
    ],
    // Add more topics as needed...
  };

  const fetchDataForQuestion = async ({ id, text }) => {
    try {
      console.log(`Fetching data for question ID: ${id}, Text: "${text}"`);
      const response = await axios.post(
        'dummy', // Replace with your actual endpoint
        { questionId: id, questionText: text },
        { headers: { 'Content-Type': 'application/json' } }
      );

      console.log('Raw response data:', response.data);
      const parsedResponse = JSON.parse(response.data.body);
      console.log('Parsed response:', parsedResponse);

      const llmAnswer = parsedResponse.answer;
      const formattedAnswer = llmAnswer.split('-').map(line => line.trim()).filter(line => line);
      console.log('Formatted answer:', formattedAnswer);

      const factualInfo = parsedResponse.factualInfo?.startsWith('http') ? parsedResponse.factualInfo : null;
      const citizenExperience = parsedResponse.citizenExperience?.startsWith('http') ? parsedResponse.citizenExperience : null;

      return {
        textualResponse: formattedAnswer.length > 0 ? formattedAnswer : ['No Answer Available'],
        factualInfo: factualInfo || 'Factual information goes here.',
        citizenExperience: citizenExperience || 'Citizen experience response goes here.',
        contextual: parsedResponse.contextual || 'Contextual information goes here.',
      };
    } catch (error) {
      console.error('Error fetching data:', error);
      return {
        textualResponse: ['No Answer Available'],
        factualInfo: null,
        citizenExperience: null,
        contextual: 'No contextual information available.',
      };
    }
  };

  const fetchAllData = async (topicId) => {
    setLoading(true);
    try {
      const questionsList = topicQuestions[topicId] || [];
      console.log(`Fetching all data for topic ID: ${topicId}`, questionsList);

      const formattedData = await Promise.all(
        questionsList.map(async (question) => {
          const answerData = await fetchDataForQuestion(question);
          console.log(`Fetched data for question: "${question.text}"`, answerData);
          return {
            question: question.text,
            answer: answerData,
          };
        })
      );

      console.log('All fetched data for current topic:', formattedData);
      setContentData(formattedData);
      setLoading(false);
    } catch (error) {
      console.error('Error fetching data for all questions:', error);
      setLoading(false);
    }
  };

  const handleQuestionSelect = (question, answer) => {
    console.log('Selected question and answer:', question, answer);
    setSelectedData((prev) => ({
      ...prev,
      [activeTopic]: {
        question,
        answer,
      },
    }));
  };

  useEffect(() => {
    console.log('Active topic changed:', activeTopic);
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
