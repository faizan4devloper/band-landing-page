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

const QuestionBlock = ({ question, answerData, onAddToChat }) => {
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

      {/* Add to Chat Button */}
      <button className={styles.addButton} onClick={() => onAddToChat(question, answerData)}>
        Add to Chat
      </button>
    </div>
  );
};

export default QuestionBlock;





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
  const [chatMessages, setChatMessages] = useState([]); // Store messages for chatbot

  // Define the questions based on the active topic
  const topicQuestions = {
    1: [
      { id: 'Q1', text: 'What are the last five years key statistics for Serpell Primary School?' },
      { id: 'Q2', text: 'What are the admission criteria and process for Serpell Primary School?' },
      { id: 'Q3', text: 'How does the school perform in standardized tests and assessments?' },
    ],
    // other topics...
  };

  const fetchDataForQuestion = async (question) => {
    const payload = {
      question_id: question.id,
      question: question.text,
    };

    try {
      const response = await axios.post('dummy', payload, { headers: { 'Content-Type': 'application/json' } });
      const parsedResponse = typeof response.data.body === 'string'
        ? JSON.parse(response.data.body)
        : response.data.body;

      const llmAnswer = parsedResponse.answer || '';
      const formattedAnswer = llmAnswer.split('-').map(line => line.trim()).filter(line => line);

      return {
        question: question.text,
        answer: {
          textualResponse: formattedAnswer.length > 0 ? formattedAnswer : ['No Answer Available'],
          factualData: parsedResponse.factualData || 'No factual information available.',
          citizenReview: parsedResponse.citizenReview || 'No citizen experience information available.',
          contextual: parsedResponse.contextual || 'No contextual information available.',
        },
      };
    } catch (error) {
      console.error("Error fetching data for question:", error);
      return {
        question: question.text,
        answer: { textualResponse: ['No Answer Available'] },
      };
    }
  };

  const fetchAllData = async (topicId) => {
    setLoading(true);
    try {
      const questionsList = topicQuestions[topicId] || [];
      const formattedData = await Promise.all(questionsList.map(fetchDataForQuestion));
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
      [activeTopic]: { question, answer },
    }));
  };

  const handleAddToChat = (question, answerData) => {
    const userMessage = { type: 'user', text: question };
    const botMessage = { type: 'bot', text: answerData.textualResponse.join(' ') };

    setChatMessages((prevMessages) => [...prevMessages, userMessage, botMessage]);
  };

  useEffect(() => {
    fetchAllData(activeTopic);
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
                onAddToChat={handleAddToChat}
              />
            </div>
          )}
        </>
      )}
    </div>
  );
};

export default MainContent;





import React, { useState, useEffect, useRef } from 'react';
import axios from 'axios';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faWandSparkles, faUser, faComments, faTimes } from '@fortawesome/free-solid-svg-icons';
import { BeatLoader } from 'react-spinners';
import styles from './Chatbot.module.css';

const Chatbot = ({ addQuestion }) => {
  const [input, setInput] = useState('');
  const [messages, setMessages] = useState([]);
  const [loading, setLoading] = useState(false);
  const [isChatVisible, setChatVisible] = useState(false); // State for visibility
  const messagesEndRef = useRef(null);

  useEffect(() => {
    // Auto-scroll to the latest message
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
      const response = await axios.post('dummy', {
        question: input,
      }, {
        headers: {
          'Content-Type': 'application/json',
        },
      });

      const parsedBody = JSON.parse(response.data.body);
      const answer = parsedBody.answer || 'No answer available for this question.';
      const source = parsedBody.source || 'No source available';

      const botMessage = {
        type: 'bot',
        text: `${answer} (Source: ${source})`,
      };

      setMessages((prevMessages) => [...prevMessages, botMessage]);

      // Automatically ask a follow-up question
      setTimeout(() => {
        const followUpQuestion = 'Would you like to ask something else?';
        const followUpMessage = {
          type: 'bot',
          text: followUpQuestion,
        };
        setMessages((prevMessages) => [...prevMessages, followUpMessage]);
      }, 1500);

    } catch (error) {
      console.error('Error fetching data:', error);
      const errorMessage = { type: 'bot', text: 'Something went wrong. Please try again later.' };
      setMessages((prevMessages) => [...prevMessages, errorMessage]);
    } finally {
      setLoading(false);
    }
  };

  const handleAddQuestion = (question, answer) => {
    addQuestion(question, answer); // Pass question and answer to the parent component
    setInput(''); // Reset input after adding the question
  };

  const toggleChatVisibility = () => {
    setChatVisible(!isChatVisible); // Toggle visibility
  };

  return (
    <div className={styles.chatContainer}>
      {/* Conversation Icon */}
      <div className={styles.iconContainer} onClick={toggleChatVisibility}>
        <FontAwesomeIcon icon={faComments} className={styles.conversationIcon} />
      </div>

      {/* Chat Window - Modal Style */}
      {isChatVisible && (
        <div className={styles.chatWindow}>
          <div className={styles.chatHeader}>
            <p className={styles.chatHeading}>Chatbot</p>
            <button className={styles.closeButton} onClick={toggleChatVisibility}>
              <FontAwesomeIcon icon={faTimes} />
            </button>
          </div>
          <div className={styles.messages}>
            {messages.map((message, index) => (
              <div
                key={index}
                className={message.type === 'user' ? styles.userMessage : styles.botMessage}
              >
                <FontAwesomeIcon
                  icon={message.type === 'user' ? faUser : faWandSparkles}
                  className={styles.icon}
                />
                <div className={styles.messageText}>
                  {message.text}
                </div>
              </div>
            ))}
            {loading && (
              <div className={styles.botMessage}>
                <FontAwesomeIcon icon={faWandSparkles} className={styles.icon} />
                <div className={styles.messageText}>
                  <BeatLoader color="#5f1ec1" size={8} />
                </div>
              </div>
            )}
            <div ref={messagesEndRef} />
          </div>

          {/* Input Form */}
          <form onSubmit={handleSubmit} className={styles.inputForm}>
            <input
              type="text"
              value={input}
              onChange={(e) => setInput(e.target.value)}
              placeholder="Ask a question..."
              className={styles.inputField}
              disabled={loading}
            />
            <button type="submit" className={styles.submitButton} title="Send">
              <FontAwesomeIcon icon={faPaperPlane} />
            </button>
          </form>

          {/* Add Question Button */}
          <button
            onClick={() => handleAddQuestion(input, 'This is where the answer will be added.')}
            className={styles.addQuestionButton}
            disabled={loading || !input}
          >
            Add Question
          </button>
        </div>
      )}
    </div>
  );
};

export default Chatbot;
