<div className="chat-wrapper">
  {/* Left Panel - Chatbot */}
  <div className="chatbot-panel">
    <div className="chat-header">
      <FontAwesomeIcon icon={faRobot} className="header-icon" />
      Insurance Claim Assist
      <span className="powered-by">Powered by Agentic AI</span>
    </div>

    <div className="upload-button-container">
      <input
        type="file"
        id="image-upload"
        onChange={(e) => handleUpload(e.target.files[0])}
        style={{ display: 'none' }}
      />
      <button
        className="btn btn-upload"
        onClick={() => document.getElementById('image-upload').click()}
      >
        <FontAwesomeIcon icon={faUpload} /> Upload
      </button>
    </div>

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
            <div className="message-text">{msg.text}</div>
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

    <div className="chat-input-container">
      <div className="input-wrapper">
        <input
          type="text"
          value={input}
          onChange={(e) => setInput(e.target.value)}
          onKeyDown={handleKeyPress}
          placeholder="Type your message..."
          className="chat-input"
        />
        <button onClick={sendMessage} className="btn btn-send">
          <FontAwesomeIcon icon={faPaperPlane} />
        </button>
      </div>

      <div className="button-group">
        <button
          onClick={toggleTraceData}
          className={`btn btn-trace ${showTracePanel ? 'active' : ''}`}
          disabled={isTraceLoading}
        >
          {isTraceLoading ? (
            <BeatLoader color="#fff" size={12} />
          ) : showTracePanel ? (
            <>
              Hide Trace
              <FontAwesomeIcon icon={faChevronRight} className="btn-icon right-chevron" />
            </>
          ) : (
            <>
              Enable Trace
              <FontAwesomeIcon icon={faChevronRight} className="btn-icon right-chevron" />
            </>
          )}
        </button>
      </div>
    </div>
  </div>

  {/* Trace Panel */}
  {showTracePanel && (
    <TracePanel
      isLoading={isLoading}
      traceData={traceData}
      isTraceEnabled={isTraceEnabled}
      onClose={() => setShowTracePanel(false)}
    />
  )}
</div>




.upload-button-container {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  padding: 10px;
  margin-bottom: 10px;
}

.btn-upload {
  background-color: #4caf50;
  color: white;
  padding: 10px 15px;
  font-size: 14px;
  border: none;
  cursor: pointer;
  border-radius: 5px;
  display: flex;
  align-items: center;
  gap: 8px;
}

.btn-upload:hover {
  background-color: #45a049;
}
