const toggleTraceData = async (index) => {
  
  setMessages((prevMessages) =>
     prevMessages.map((msg, i) => ({
      ...msg,
      showTracePanel: i === index ? !msg.showTracePanel : false, // Only toggle the clicked one
    }))
      
    
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
  }, 1000); 
  if (showTracePanel) {
    setShowTracePanel(false);
    return;
  }
  setIsTraceLoading(true); // Set trace loading state
  try {
    const response = await fetch(traceApiEndpoint, {
      method: 'GET',
      headers: { Accept: 'application/json' },
    });

    if (!response.ok) throw new Error('Trace fetch failed');

    const data = await response.json();
    setTraceData(data);
    setIsTraceEnabled(true);
    setShowTracePanel(true);
  } catch (error) {
    console.error('Trace error:', error);
    setTraceData([]);
    setShowTracePanel(false);
  } finally {
    setIsTraceLoading(false); // Reset trace loading state
  }
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
