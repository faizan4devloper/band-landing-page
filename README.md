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
    const followUpQuestions = parsedBody.followUpQuestions || [];

    const botMessage = {
      type: 'bot',
      text: `${answer} (Source: ${source})`,
      followUpQuestions: followUpQuestions.slice(0, 5) // Limit to top 5 questions
    };
    setMessages((prevMessages) => [...prevMessages, botMessage]);

  } catch (error) {
    console.error('Error fetching data:', error);
    const errorMessage = { type: 'bot', text: 'Something went wrong. Please try again later.' };
    setMessages((prevMessages) => [...prevMessages, errorMessage]);
  } finally {
    setLoading(false);
  }
};






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
      
      {/* Follow-Up Questions for Bot Messages */}
      {message.type === 'bot' && message.followUpQuestions && (
        <div className={styles.followUpQuestions}>
          {message.followUpQuestions.map((question, idx) => (
            <button
              key={idx}
              onClick={() => handleFollowUpClick(question)}
              className={styles.followUpButton}
            >
              {question}
            </button>
          ))}
        </div>
      )}
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




const handleFollowUpClick = (question) => {
  setInput(question); // Populate input with the follow-up question
  handleSubmit({ preventDefault: () => {} }); // Trigger submission
};





.followUpQuestions {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin-top: 8px;
}

.followUpButton {
  background-color: var(--user-message-background);
  border: none;
  color: #333;
  padding: 4px 8px;
  border-radius: 8px;
  font-size: 12px;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

.followUpButton:hover {
  background-color: var(--primary-color);
  color: #fff;
}
