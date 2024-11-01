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

  // Define the questions for each topic
  const topicQuestions = {
    1: [
      { id: 'q1', text: 'What are the last five years key statistics for Serpell Primary School?' },
      { id: 'q2', text: 'What are the admission criteria and process for Serpell Primary School?' },
      { id: 'q3', text: 'How does the school perform in standardized tests and assessments?' },
    ],
    2: [
      { id: 'q4', text: 'What’s the curriculum at Serpell Primary School?' },
      { id: 'q5', text: 'How is the curriculum structured across different year levels?' },
      { id: 'q6', text: 'What specialist programs are offered at Serpell Primary School?' },
    ],
    3: [
      { id: 'q7', text: 'How does the school engage with the broader community?' },
      { id: 'q8', text: 'How can parents get involved in the school community?' },
      { id: 'q9', text: 'What support services are available for students with special needs at Serpell Primary School?' },
    ],
  };

  // Ensure URLs are correctly formatted with .png extension
  const ensureFullUrl = (url) => {
    if (!url || url === "https://") {
      return 'https://your-default-placeholder-image-url.png'; // Use a placeholder URL or keep null
    }
    return url.startsWith("https://") ? url : `https://${url}`;
  };

  // Fetch data for each question
  const fetchDataForQuestion = async ({ id, text }) => {
    try {
      const response = await axios.post(
        'dummy', // Replace with your actual endpoint
        { questionId: id, questionText: text },
        { headers: { 'Content-Type': 'application/json' } }
      );

      const parsedResponse = JSON.parse(response.data.body);
      const llmAnswer = parsedResponse.answer;
      const formattedAnswer = llmAnswer.split('-').map(line => line.trim()).filter(line => line);

      const factualInfo = ensureFullUrl(parsedResponse.factualInfo);
      const citizenExperience = ensureFullUrl(parsedResponse.citizenExperience);

      return {
        textualResponse: formattedAnswer.length > 0 ? formattedAnswer : ['No Answer Available'],
        factualInfo: factualInfo,
        citizenExperience: citizenExperience,
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

      const formattedData = await Promise.all(
        questionsList.map(async (question) => {
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








import React, { useState } from 'react';
import styles from './QuestionBlock.module.css';

const parseLinks = (text) => {
  const urlPattern = /(https?:\/\/[^\s]+)/g;
  return text.split(urlPattern).map((part, index) =>
    urlPattern.test(part) ? (
      <a key={index} href={part} target="_blank" rel="noopener noreferrer" className={styles.link}>
        {part}
      </a>
    ) : (
      part
    )
  );
};

const QuestionBlock = ({ question, answerData }) => {
  const [showFullTextual, setShowFullTextual] = useState(false);
  const [showFullCitizen, setShowFullCitizen] = useState(false);
  const [showFullFactual, setShowFullFactual] = useState(false);
  const [showFullContextual, setShowFullContextual] = useState(false);

  const renderResponsePoints = (responseArray, isExpanded) => {
    const arrayToRender = Array.isArray(responseArray) ? responseArray : [responseArray];
    return (
      <ul className={styles.responseList}>
        {(isExpanded ? arrayToRender : arrayToRender.slice(0, 2)).map((item, index) => (
          <li key={index} className={styles.responseItem}>
            {parseLinks(item)}
          </li>
        ))}
      </ul>
    );
  };

  const renderImage = (imageUrl, altText) => (
    imageUrl && imageUrl !== 'https://' // Ensure the image URL is valid
      ? <img src={imageUrl} alt={altText} className={styles.responseImage} />
      : null // Optionally add a placeholder image here
  );

  return (
    <div className={styles.questionBlock}>
      <div className={styles.question}>{question}</div>

      {/* Textual Response */}
      <div className={`${styles.responseSection} ${showFullTextual ? styles.expanded : ''}`}>
        <p><strong>Textual Response:</strong></p>
        {renderResponsePoints(answerData.textualResponse, showFullTextual)}
        {renderImage(answerData.textualImage, "Textual Response Image")}
        {answerData.textualResponse && answerData.textualResponse.length > 2 && (
          <button className={styles.seeMoreButton} onClick={() => setShowFullTextual(!showFullTextual)}>
            {showFullTextual ? 'See Less' : 'See More'}
          </button>
        )}
      </div>

      {/* Citizen Experience */}
      <div className={`${styles.responseSection} ${showFullCitizen ? styles.expanded : ''}`}>
        <p><strong>Citizen Experience:</strong></p>
        {renderResponsePoints([answerData.citizenExperience], showFullCitizen)}
        {renderImage(answerData.citizenExperience, "Citizen Experience Image")}
        {answerData.citizenExperience && answerData.citizenExperience.length > 50 && (
          <button className={styles.seeMoreButton} onClick={() => setShowFullCitizen(!showFullCitizen)}>
            {showFullCitizen ? 'See Less' : 'See More'}
          </button>
        )}
      </div>

      {/* Factual Info */}
      <div className={`${styles.responseSection} ${showFullFactual ? styles.expanded : ''}`}>
        <p><strong>Factual Info:</strong></p>
        {renderResponsePoints([answerData.factualInfo], showFullFactual)}
        {renderImage(answerData.factualInfo, "Factual Info Image")}
        {answerData.factualInfo && answerData.factualInfo.length > 50 && (
          <button className={styles.seeMoreButton} onClick={() => setShowFullFactual(!showFullFactual)}>
            {showFullFactual ? 'See Less' : 'See More'}
          </button>
        )}
      </div>

      {/* Contextual */}
      <div className={`${styles.responseSection} ${showFullContextual ? styles.expanded : ''}`}>
        <p><strong>Contextual:</strong></p>
        {renderResponsePoints([answerData.contextual], showFullContextual)}
        {answerData.contextual && answerData.contextual.length > 50 && (
          <button className={styles.seeMoreButton} onClick={() => setShowFullContextual(!showFullContextual)}>
            {showFullContextual ? 'See Less' : 'See More'}
          </button>
        )}
      </div>
    </div>
  );
};

export default QuestionBlock;
