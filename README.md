import React, { useState, useEffect, useRef } from 'react';
import axios from 'axios';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faWandSparkles, faUser, faComments, faTimes } from '@fortawesome/free-solid-svg-icons';
import { BeatLoader } from 'react-spinners';
import styles from './Chatbot.module.css';

const Chatbot = ({ chatbotMessages, setChatbotMessages }) => {
  const [input, setInput] = useState('');
  const [messages, setMessages] = useState(chatbotMessages || []);
  const [loading, setLoading] = useState(false);
  const [isChatVisible, setChatVisible] = useState(false); // State for visibility

  const messagesEndRef = useRef(null);

  useEffect(() => {
    // Auto-scroll to the latest message
    if (messagesEndRef.current) {
      messagesEndRef.current.scrollIntoView({ behavior: 'smooth' });
    }
  }, [messages]);

  useEffect(() => {
    setMessages(chatbotMessages || []);
  }, [chatbotMessages]);

  const handleSubmit = async (e) => {
    e.preventDefault();
    if (input.trim() === '') return;

    // User's message
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

      // Pass the selected question and answer to the parent (MainContent)
      setChatbotMessages(prevMessages => [
        ...prevMessages,
        { type: 'user', text: input },
        { type: 'bot', text: botMessage.text },
      ]);

    } catch (error) {
      console.error('Error fetching data:', error);
      const errorMessage = { type: 'bot', text: 'Something went wrong. Please try again later.' };
      setMessages((prevMessages) => [...prevMessages, errorMessage]);
    } finally {
      setLoading(false);
    }
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
            <button className={styles.closeButton} onClick={toggleChatVisibility}><FontAwesomeIcon icon={faTimes} /></button>
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
                <div className={styles.messageContent}>
                  <p>{message.text}</p>
                </div>
              </div>
            ))}
            {loading && (
              <div className={styles.loaderContainer}>
                <BeatLoader color="rgb(15, 95, 220)" loading={loading} size={10} />
              </div>
            )}
            <div ref={messagesEndRef} />
          </div>

          {/* Input Section */}
          <form onSubmit={handleSubmit} className={styles.inputForm}>
            <input
              type="text"
              value={input}
              onChange={(e) => setInput(e.target.value)}
              placeholder="Ask me something..."
              className={styles.inputField}
            />
            <button type="submit" className={styles.submitButton}>
              <FontAwesomeIcon icon={faPaperPlane} />
            </button>
          </form>
        </div>
      )}
    </div>
  );
};

export default Chatbot;








import React, { useState, useEffect } from 'react';
import axios from 'axios';
import PropagateLoader from 'react-spinners/PropagateLoader';
import FaqDropdown from './FaqDropdown';
import QuestionBlock from './QuestionBlock';
import styles from './MainContent.module.css';

const MainContent = ({ activeTopic, setChatbotMessages }) => {
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

    // Pass the selected question and answer to the chatbot
    setChatbotMessages(prevMessages => [
      ...prevMessages,
      { type: 'user', text: question },
      { type: 'bot', text: answer.textualResponse.join('\n') },
    ]);
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
