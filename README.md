const sendMessage = async () => {
  if (input.trim() === '') return;

  // Add user message
  const userMessage = { text: input, sender: 'user' };
  setMessages((prevMessages) => [...prevMessages, userMessage]);
  setInput('');
  setLoading(true);

  // Add a loading indicator as a bot message before the actual response
  setMessages((prevMessages) => [
    ...prevMessages,
    { text: '', sender: 'bot', loading: true },
  ]);

  try {
    // Use Axios to fetch the bot response
    const response = await axios.post('https://your-api-id.execute-api.region.amazonaws.com/your-stage', { message: input });
    const botMessage = { text: response.data.message, sender: 'bot' };

    setMessages((prevMessages) => {
      const updatedMessages = [...prevMessages];
      const lastLoadingMessageIndex = updatedMessages.findIndex(msg => msg.loading);

      if (lastLoadingMessageIndex !== -1) {
        // Replace the loading message with the bot's response
        updatedMessages[lastLoadingMessageIndex] = botMessage;
      }

      return updatedMessages;
    });
  } catch (error) {
    console.error('Error fetching bot response:', error);
    // Add an error message or fallback response
    const errorMessage = { text: 'Sorry, there was an error getting a response.', sender: 'bot' };

    setMessages((prevMessages) => {
      const updatedMessages = [...prevMessages];
      const lastLoadingMessageIndex = updatedMessages.findIndex(msg => msg.loading);

      if (lastLoadingMessageIndex !== -1) {
        // Replace the loading message with the error message
        updatedMessages[lastLoadingMessageIndex] = errorMessage;
      }

      return updatedMessages;
    });
  } finally {
    setLoading(false);
  }
};





import React, { useState, useEffect } from 'react';
import axios from 'axios';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faRobot, faTimes } from '@fortawesome/free-solid-svg-icons';
import { faUser } from '@fortawesome/free-regular-svg-icons';
import { BeatLoader } from 'react-spinners';
import styles from './Chatbot.module.css';

const Chatbot = () => {
  const [isOpen, setIsOpen] = useState(false);
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    if (isOpen) {
      setMessages([{ text: 'Hello! How can I assist you today?', sender: 'bot' }]);
    }
  }, [isOpen]);

  const toggleChatbot = () => {
    setIsOpen(!isOpen);
  };

  const sendMessage = async () => {
    if (input.trim() === '') return;

    // Add user message
    const userMessage = { text: input, sender: 'user' };
    setMessages((prevMessages) => [...prevMessages, userMessage]);
    setInput('');
    setLoading(true);

    // Add a loading indicator as a bot message before the actual response
    setMessages((prevMessages) => [
      ...prevMessages,
      { text: '', sender: 'bot', loading: true },
    ]);

    try {
      // Use Axios to fetch the bot response
      const response = await axios.post('xyz', { message: input }); // Replace 'xyz' with your API Gateway endpoint
      const botMessage = { text: response.data.message, sender: 'bot' };

      setMessages((prevMessages) => {
        const updatedMessages = [...prevMessages];
        const lastLoadingMessageIndex = updatedMessages.findIndex(msg => msg.loading);

        if (lastLoadingMessageIndex !== -1) {
          // Replace the loading message with the bot's response
          updatedMessages[lastLoadingMessageIndex] = botMessage;
        }

        return updatedMessages;
      });
    } catch (error) {
      console.error('Error fetching bot response:', error);
      // Add an error message or fallback response
      const errorMessage = { text: 'Sorry, there was an error getting a response.', sender: 'bot' };

      setMessages((prevMessages) => {
        const updatedMessages = [...prevMessages];
        const lastLoadingMessageIndex = updatedMessages.findIndex(msg => msg.loading);

        if (lastLoadingMessageIndex !== -1) {
          // Replace the loading message with the error message
          updatedMessages[lastLoadingMessageIndex] = errorMessage;
        }

        return updatedMessages;
      });
    } finally {
      setLoading(false);
    }
  };

  return (
    <>
      <div className={styles.chatbotIcon} onClick={toggleChatbot}>
        <FontAwesomeIcon icon={faRobot} />
      </div>

      {isOpen && (
        <div className={styles.chatbotContainer}>
          <div className={styles.chatbotHeader}>
            <button onClick={toggleChatbot} className={styles.closeButton}>
              <FontAwesomeIcon icon={faTimes} />
            </button>
          </div>

          <div className={styles.chatbotMessages}>
            {messages.map((message, index) => (
              <div
                key={index}
                className={
                  message.sender === 'user' ? styles.userMessage : styles.botMessage
                }
              >
                <FontAwesomeIcon
                  icon={message.sender === 'user' ? faUser : faRobot}
                  className={styles.icon}
                />
                <div className={styles.messageText}>
                  {message.loading ? (
                    <BeatLoader color="#5f1ec1" size={8} />
                  ) : (
                    message.text
                  )}
                </div>
              </div>
            ))}
          </div>

          <div className={styles.chatbotInput}>
            <input
              type="text"
              value={input}
              onChange={(e) => setInput(e.target.value)}
              onKeyDown={(e) => {
                if (e.key === "Enter") {
                  sendMessage();
                }
              }}
              placeholder="Type your message..."
              disabled={loading}
            />
            <button onClick={sendMessage} disabled={loading} className={styles.sendButton}>
              <FontAwesomeIcon icon={faPaperPlane} />
            </button>
          </div>
        </div>
      )}
    </>
  );
};

export default Chatbot;









const sendMessage = async () => {
  if (input.trim() === '') return;

  // Add user message
  const userMessage = { text: input, sender: 'user' };
  setMessages((prevMessages) => [...prevMessages, userMessage]);
  setInput('');
  setLoading(true);

  // Add a loading indicator as a bot message before the actual response
  setMessages((prevMessages) => [
    ...prevMessages,
    { text: '', sender: 'bot', loading: true },
  ]);

  try {
    // Use Axios to fetch the bot response
    const response = await axios.post('xyz', { message: input });
    const botMessage = { text: response.data.message, sender: 'bot' };

    setMessages((prevMessages) => {
      const updatedMessages = [...prevMessages];
      const lastLoadingMessageIndex = updatedMessages.findIndex(msg => msg.loading);

      if (lastLoadingMessageIndex !== -1) {
        // Replace the loading message with the bot's response
        updatedMessages[lastLoadingMessageIndex] = botMessage;
      }

      return updatedMessages;
    });
  } catch (error) {
    console.error('Error fetching bot response:', error);
    // Add an error message or fallback response
    const errorMessage = { text: 'Sorry, there was an error getting a response.', sender: 'bot' };

    setMessages((prevMessages) => {
      const updatedMessages = [...prevMessages];
      const lastLoadingMessageIndex = updatedMessages.findIndex(msg => msg.loading);

      if (lastLoadingMessageIndex !== -1) {
        // Replace the loading message with the error message
        updatedMessages[lastLoadingMessageIndex] = errorMessage;
      }

      return updatedMessages;
    });
  } finally {
    setLoading(false);
  }
};






import React, { useState, useEffect } from 'react';
import axios from 'axios';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faPaperPlane, faRobot, faTimes } from '@fortawesome/free-solid-svg-icons';
import { faUser } from '@fortawesome/free-regular-svg-icons';
import { BeatLoader } from 'react-spinners';
import styles from './Chatbot.module.css';

const Chatbot = () => {
  const [isOpen, setIsOpen] = useState(false);
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    if (isOpen) {
      setMessages([{ text: 'Hello! How can I assist you today?', sender: 'bot' }]);
    }
  }, [isOpen]);

  const toggleChatbot = () => {
    setIsOpen(!isOpen);
  };

  // const sendMessage = () => {
  //   if (input.trim() === '') return;

  //   // Add user message
  //   const userMessage = { text: input, sender: 'user' };
  //   setMessages([...messages, userMessage]);
  //   setInput('');
  //   setLoading(true);

  //   // Add a loading indicator as a bot message before the actual response
  //   setMessages((prevMessages) => [
  //     ...prevMessages,
  //     { text: '', sender: 'bot', loading: true },
  //   ]);

  //   // Simulate bot response after a delay
  //   setTimeout(() => {
  //     const botMessage = { text: 'This is a bot response.', sender: 'bot' };
  //     setMessages((prevMessages) => {
  //       // Remove the loading indicator and add the actual bot message
  //       return prevMessages.map((msg) =>
  //         msg.loading ? botMessage : msg
  //       );
  //     });
  //     setLoading(false);
  //   }, 1000);
  // };
  
  const sendMessage = async () => {
    if (input.trim() === '') return;

    // Add user message
    const userMessage = { text: input, sender: 'user' };
    setMessages([...messages, userMessage]);
    setInput('');
    setLoading(true);

    // Add a loading indicator as a bot message before the actual response
    setMessages((prevMessages) => [
      ...prevMessages,
      { text: '', sender: 'bot', loading: true },
    ]);

    try {
      // Use Axios to fetch the bot response
      const response = await axios.post('xyz', { message: input });
      const botMessage = { text: response.data.message, sender: 'bot' };

      setMessages((prevMessages) => {
        // Remove the loading indicator and add the actual bot message
        return prevMessages.map((msg) =>
          msg.loading ? botMessage : msg
        );
      });
    } catch (error) {
      console.error('Error fetching bot response:', error);
      // Add an error message or fallback response
      const errorMessage = { text: 'Sorry, there was an error getting a response.', sender: 'bot' };
      setMessages((prevMessages) => {
        return prevMessages.map((msg) =>
          msg.loading ? errorMessage : msg
        );
      });
    } finally {
      setLoading(false);
    }
  };

  
  
  return (
    <>
      <div className={styles.chatbotIcon} onClick={toggleChatbot}>
        <FontAwesomeIcon icon={faRobot} />
      </div>
      
      {isOpen && (
        <div className={styles.chatbotContainer}>
          <div className={styles.chatbotHeader}>
            <button onClick={toggleChatbot} className={styles.closeButton}>
              <FontAwesomeIcon icon={faTimes} />
            </button>
          </div>

          <div className={styles.chatbotMessages}>
            {messages.map((message, index) => (
              <div
                key={index}
                className={
                  message.sender === 'user' ? styles.userMessage : styles.botMessage
                }
              >
                <FontAwesomeIcon
                  icon={message.sender === 'user' ? faUser : faRobot}
                  className={styles.icon}
                />
                <div className={styles.messageText}>
                  {message.loading ? (
                    <BeatLoader color="#5f1ec1" size={8} />
                  ) : (
                    message.text
                  )}
                </div>
              </div>
            ))}
          </div>

          <div className={styles.chatbotInput}>
            <input
              type="text"
              value={input}
    onChange={(e) => setInput(e.target.value)}
    onKeyDown={(e) => {
      if (e.key === "Enter") {
        sendMessage();
      }
    }}
              placeholder="Type your message..."
              disabled={loading}
            />
            <button onClick={sendMessage} disabled={loading} className={styles.sendButton}>
                <FontAwesomeIcon icon={faPaperPlane} />
            </button>
          </div>
        </div>
      )}
    </>
  );
};

export default Chatbot;
