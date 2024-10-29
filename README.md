:root {
  --primary-color: #5f1ec1;
  --secondary-color: rgba(15, 95, 220, 1);
  --background-color: #ffffff;
  --chat-text-color: #fff;
  --message-background: #f1f1f1;
  --user-message-background: #d1e7ff;
  --input-background: #ffffff;
  --input-border: #ddd;
  --loading-color: #5f1ec1;
}

/* Dark Theme */
[data-theme="dark"] {
  --primary-color: #9d66f5;
  --background-color: #1a1a2e;
  --message-background: #333;
  --user-message-background: #444;
  --input-background: #444;
  --input-border: #666;
  --loading-color: #c1a1f2;
}

.chatContainer {
    position: fixed;
    bottom: 20px;
    right: 20px;
    z-index: 1000;
    animation: fadeIn 0.5s; /* Animation for the container */
}

.iconContainer {
  cursor: pointer;  
  padding: 14px;
  border-radius: 30%;
  background: linear-gradient(90deg, var(--primary-color) 0%, var(--secondary-color) 100%);
  transition: transform 0.3s ease;
}

.iconContainer:hover {
  transform: scale(1.1); /* Scale effect on hover */
}

.chatWindow {
  position: fixed;
  bottom: 80px;
  right: 20px;
  width: 320px;
  height: 420px;
  background-color: var(--background-color);
  border-radius: 8px;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.3);
  display: flex;
  flex-direction: column;
  transition: all 0.3s ease;
  animation: slideIn 0.5s; /* Animation for chat window */
}

.messages {
  flex: 1;
  padding: 10px;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  transition: opacity 0.5s; /* Smooth opacity transition */
}

.userMessage,
.botMessage {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
  animation: slideIn 0.3s; /* Slide-in effect for messages */
}

.userMessage {
  justify-content: flex-end;
}

.messageText {
  max-width: 75%;
  background-color: var(--message-background);
  padding: 10px;
  font-size: 12px;
  border-radius: 10px;
  color: #333;
  word-wrap: break-word;
  white-space: pre-wrap;
  transition: background-color 0.3s; /* Smooth background transition */
}

.userMessage .messageText {
  background-color: var(--user-message-background);
}

.inputField {
  flex: 1;
  padding: 10px;
  border: 1px solid var(--input-border);
  border-radius: 4px;
  color: #000;
  outline: none;
  background-color: var(--input-background);
  transition: border-color 0.3s; /* Transition for input border */
}

.inputField:focus {
  border-color: var(--primary-color); /* Change border color on focus */
}

.submitButton {
  background: linear-gradient(90deg, var(--primary-color) 0%, var(--secondary-color) 100%);
  color: var(--chat-text-color);
  border: none;
  padding: 10px;
  border-radius: 4px;
  margin-left: 10px;
  cursor: pointer;
  transition: transform 0.2s; /* Transition for button */
}

.submitButton:hover {
  transform: scale(1.05); /* Scale effect on hover */
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

@keyframes slideIn {
  from {
    transform: translateY(20px);
    opacity: 0;
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}



const Chatbot = () => {
  const [input, setInput] = useState('');
  const [messages, setMessages] = useState([]);
  const [loading, setLoading] = useState(false);
  const [isChatVisible, setChatVisible] = useState(false);
  const [typing, setTyping] = useState(false); // New state for typing indication

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
    setTyping(true); // Set typing to true

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
    } catch (error) {
      console.error('Error fetching data:', error);
      const errorMessage = { type: 'bot', text: 'Something went wrong. Please try again later.' };
      setMessages((prevMessages) => [...prevMessages, errorMessage]);
    } finally {
      setLoading(false);
      setTyping(false); // Set typing to false
    }
  };

  const toggleChatVisibility = () => {
    setChatVisible(!isChatVisible);
  };

  return (
    <div className={styles.chatContainer}>
      <div className={styles.iconContainer} onClick={toggleChatVisibility}>
        <FontAwesomeIcon icon={faComments} className={styles.conversationIcon} />
      </div>

      {isChatVisible && (
        <div className={styles.chatWindow}>
          <div className={styles.chatHeader}>
            <p className={styles.chatHeading}>Chatbot</p>
            <button className={styles.closeButton} onClick={toggleChatVisibility}><FontAwesomeIcon icon={faTimes}/></button>
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
            {typing && (
              <div className={styles.botMessage}>
                <FontAwesomeIcon icon={faWandSparkles} className={styles.icon} />
                <div className={styles.messageText}>
                  Typing...
                </div>
              </div>
            )}
            {loading && (
              <div className={styles.botMessage}>
                <FontAwesomeIcon icon={faWandSparkles} className={styles.icon} />
                <div className={styles.messageText}>
                  <BeatLoader color={styles.loadingColor} size={8} />
                </div>
              </div>
            )}
            <div ref={messagesEndRef} />
          </div>

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
        </div>
      )}
    </div>
  );
};
