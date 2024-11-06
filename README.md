// MainContent.js

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
  const [clickedQuestion, setClickedQuestion] = useState(null); // To track which question was clicked

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

  // Fetch data for each question
  const fetchDataForQuestion = async ({ id, text }) => {
    try {
      const response = await axios.post(
        'dummy', // Replace with your actual endpoint
        { questionId: id, questionText: text },
        { headers: { 'Content-Type': 'application/json' } }
      );

      const parsedResponse = JSON.parse(response.data.body);
      console.log('Parsed Response:', parsedResponse);

      const llmAnswer = parsedResponse.answer || '';
      const formattedAnswer = llmAnswer.split('-').map(line => line.trim()).filter(line => line);

      const factualData = parsedResponse.factualData && parsedResponse.factualData.startsWith("http")
        ? parsedResponse.factualData
        : null;

      const citizenReview = parsedResponse.citizenReview && parsedResponse.citizenReview.startsWith("http")
        ? parsedResponse.citizenReview
        : null;

      console.log('Factual Data URL:', factualData);
      console.log('Citizen Review URL:', citizenReview);

      return {
        textualResponse: formattedAnswer.length > 0 ? formattedAnswer : ['No Answer Available'],
        factualData: factualData || 'No factual information available.',
        citizenReview: citizenReview || 'No citizen experience information available.',
        contextual: parsedResponse.contextual || 'No contextual information available.',
      };
    } catch (error) {
      console.error('Error fetching data:', error);
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

  const handleClick = (question) => {
    setClickedQuestion(question); // Track clicked question
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
          {contentData.map((data) => (
            <div
              key={data.question}
              className={`${styles.responseSection} ${clickedQuestion === data.question ? styles.clicked : ''}`}
              onClick={() => handleClick(data.question)}
            >
              <p>{data.question}</p>
              {clickedQuestion === data.question && (
                <QuestionBlock
                  question={data.question}
                  answerData={data.answer}
                />
              )}
            </div>
          ))}
        </>
      )}
    </div>
  );
};

export default MainContent;




// QuestionBlock.js

import React from 'react';
import styles from './QuestionBlock.module.css';

// Utility function to convert URLs in text to anchor tags
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
  const renderResponsePoints = (responseArray) => {
    const arrayToRender = Array.isArray(responseArray) ? responseArray : [responseArray];
    return (
      <ul className={styles.responseList}>
        {arrayToRender.map((item, index) => (
          <li key={index} className={styles.responseItem}>
            {parseLinks(item)}
          </li>
        ))}
      </ul>
    );
  };

  const renderImage = (imageUrl, altText) =>
    imageUrl ? (
      <img src={imageUrl} alt={altText} className={styles.responseImage} />
    ) : null;

  return (
    <div className={styles.questionBlock}>
      <div className={styles.question}>{question}</div>

      {/* Textual Response */}
      <div className={styles.responseSection}>
        <p><strong>Textual Response:</strong></p>
        {renderResponsePoints(answerData.textualResponse)}
        {renderImage(answerData.textualImage, "Textual Response Image")}
      </div>

      {/* Citizen Experience */}
      <div className={styles.responseSection}>
        <p><strong>Citizen Experience:</strong></p>
        {renderImage(answerData.citizenReview, "Citizen Experience Image")}
      </div>

      {/* Factual Info */}
      <div className={styles.responseSection}>
        <p><strong>Factual Info:</strong></p>
        {renderImage(answerData.factualData, "Factual Info Image")}
      </div>

      {/* Contextual */}
      <div className={styles.responseSection}>
        <p><strong>Contextual:</strong></p>
        {renderResponsePoints([answerData.contextual])}
        {renderImage(answerData.contextualImage, "Contextual Info Image")}
      </div>
    </div>
  );
};

export default QuestionBlock;



// Chatbot.js

import React, { useState, useEffect, useRef } from 'react';
import axios from 'axios';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faWandSparkles, faUser, faComments, faTimes } from '@fortawesome/free-solid-svg-icons';
import { BeatLoader } from 'react-spinners';
import styles from './Chatbot.module.css';

const Chatbot = () => {
  const [input, setInput] = useState('');
  const [messages, setMessages] = useState([]);
  const [loading, setLoading] = useState(false);
  const [isChatVisible, setChatVisible] = useState(false);

  const messagesEndRef = useRef(null);

  useEffect(() => {
    if (messagesEndRef.current) {
      messagesEndRef.current.scrollIntoView({ behavior: 'smooth' });
    }
  }, [messages]);

  const handleSubmit = async (e) => {
    e.preventDefault();
    if (input.trim() === '') return;

    const userMessage = { type: 'user', text: input };
    setMessages((prevMessages) => [...prevMessages, userMessage]);
    setInput('');
    setLoading(true);

    try {
      const response = await axios.post('dummy', { question: input }, { headers: { 'Content-Type': 'application/json' } });

      const parsedBody = JSON.parse(response.data.body);
      const botMessage = { type: 'bot', text: parsedBody.answer || 'Sorry, I couldn\'t understand that.' };
      setMessages((prevMessages) => [...prevMessages, botMessage]);
    } catch (error) {
      setMessages((prevMessages) => [
        ...prevMessages,
        { type: 'bot', text: 'Sorry, there was an issue with the request.' },
      ]);
    } finally {
      setLoading(false);
    }
  };

  const toggleChatVisibility = () => setChatVisible(!isChatVisible);

  return (
    <div className={styles.chatbotWrapper}>
      <div className={`${styles.chatbot} ${isChatVisible ? styles.visible : ''}`}>
        <div className={styles.chatHeader}>
          <span>Chatbot</span>
          <button className={styles.closeButton} onClick={toggleChatVisibility}>
            <FontAwesomeIcon icon={faTimes} />
          </button>
        </div>
        <div className={styles.chatBody}>
          <div className={styles.messages}>
            {messages.map((msg, index) => (
              <div key={index} className={msg.type === 'user' ? styles.userMessage : styles.botMessage}>
                <FontAwesomeIcon icon={msg.type === 'user' ? faUser : faComments} />
                <span>{msg.text}</span>
              </div>
            ))}
            {loading && (
              <div className={styles.loading}>
                <BeatLoader color="rgb(15, 95, 220)" loading={loading} size={8} />
              </div>
            )}
            <div ref={messagesEndRef} />
          </div>
        </div>
        <div className={styles.chatFooter}>
          <input
            type="text"
            value={input}
            onChange={(e) => setInput(e.target.value)}
            placeholder="Ask a question..."
          />
          <button onClick={handleSubmit} disabled={loading}>
            <FontAwesomeIcon icon={faPaperPlane} />
          </button>
        </div>
      </div>
      <button className={styles.toggleChatButton} onClick={toggleChatVisibility}>
        <FontAwesomeIcon icon={faWandSparkles} />
      </button>
    </div>
  );
};

export default Chatbot;
