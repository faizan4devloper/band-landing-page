const [messages, setMessages] = useState([
  { text: 'Hello, how can I help you?', type: 'bot', isTraceLoading: false, showTracePanel: false },
  { text: 'What is your question?', type: 'bot', isTraceLoading: false, showTracePanel: false },
]);




const toggleTraceData = (index) => {
  setMessages((prevMessages) =>
    prevMessages.map((msg, i) =>
      i === index
        ? { ...msg, isTraceLoading: true } // Set loading to true for the selected message
        : msg
    )
  );

  // Simulate loading time
  setTimeout(() => {
    setMessages((prevMessages) =>
      prevMessages.map((msg, i) =>
        i === index
          ? {
              ...msg,
              isTraceLoading: false,
              showTracePanel: !msg.showTracePanel, // Toggle trace panel for the selected message
            }
          : msg
      )
    );
  }, 1000); // Adjust delay as needed
};







<div className="chat-messages">
  <AnimatePresence>
    {messages.map((msg, index) => (
      <motion.div
        key={index}
        className={`message ${msg.type === 'user' ? 'user-message' : 'bot-message'}`}
        initial={{ opacity: 0, y: 20 }}
        animate={{ opacity: 1, y: 0 }}
        transition={{ duration: 0.3 }}
      >
        <div className="message-icon">
          <FontAwesomeIcon icon={msg.type === 'user' ? faUser : faRobot} className={`${msg.type}-icon`} />
        </div>
        <div className="message-text">
          {msg.text}

          {msg.type === 'bot' && (
            <span className="trace-button-container">
              <button
                onClick={() => toggleTraceData(index)} // Pass index here
                className={`btn btn-trace ${msg.showTracePanel ? 'active' : ''}`}
                disabled={msg.isTraceLoading}
              >
                {msg.isTraceLoading ? (
                  <BeatLoader color="#fff" size={12} />
                ) : msg.showTracePanel ? (
                  'Hide Trace'
                ) : (
                  'Enable Trace'
                )}
              </button>
            </span>
          )}
        </div>
      </motion.div>
    ))}
  </AnimatePresence>

  {typing && (
    <div className="typing-indicator">
      <BeatLoader color="#785ce5" size={12} />
    </div>
  )}
  <div ref={messagesEndRef} />
</div>
