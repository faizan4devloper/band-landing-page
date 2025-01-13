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
    <div className="input-icons">
      <button 
        onClick={sendMessage} 
        className="btn btn-send"
      >
        <FontAwesomeIcon icon={faPaperPlane} />
      </button>
    </div>
  </div>
  <div className="button-group">
    <input
      type="file"
      id="image-upload"
      onChange={(e) => handleUpload(e.target.files[0])}
      style={{ display: 'none' }}
    />
    <button 
      onClick={() => document.getElementById('image-upload').click()}
      className="btn btn-secondary"
    >
      <FontAwesomeIcon icon={faUpload} className="btn-icon" />
      Upload
    </button>
    <button 
      onClick={fetchTraceData} 
      className="btn btn-trace"
      disabled={isLoading}
    >
      {isLoading ? (
        <BeatLoader color="#fff" size={15} />
      ) : (
        <>
          <FontAwesomeIcon icon={faRedo} className="btn-icon" />
          {isTraceEnabled ? 'Refresh Trace' : 'Enable Trace'}
        </>
      )}
    </button>
  </div>
</div>




.chat-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  padding: 20px;
  background-color: #f5f5f5;
}

.chat-container {
  width: 100%;
  max-width: 960px;
  background-color: #ffffff;
  border-radius: 16px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  display: flex;
  flex-direction: row;
  max-height: 700px;
}

.chat-content {
  display: flex;
  flex: 1;
  overflow: hidden;
}

.chat-messages-container {
  flex: 2;
  display: flex;
  flex-direction: column;
  padding: 20px;
}

.chat-header {
  background-color: #5c6bc0;
  color: #fff;
  padding: 15px;
  text-align: center;
  font-weight: 600;
  border-radius: 10px;
  margin-bottom: 15px;
}

.chat-messages {
  flex: 1;
  overflow-y: auto;
  background-color: #f9f9fc;
  padding: 15px;
  border-radius: 10px;
}

.message {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.user-message {
  justify-content: flex-start;
}

.bot-message {
  justify-content: flex-end;
}

.message-icon {
  margin-right: 10px;
}

.message-text {
  background-color: #e1f5fe;
  padding: 10px 15px;
  border-radius: 10px;
  font-size: 16px;
  max-width: 70%;
  word-wrap: break-word;
}

.user-message .message-text {
  background-color: #c5e1f7;
}

.bot-message .message-text {
  background-color: #bbdefb;
}

.chat-input-container {
  margin-top: 10px;
}

.input-wrapper {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.chat-input {
  flex: 1;
  padding: 10px;
  border-radius: 8px;
  border: 1px solid #e0e0e0;
  margin-right: 10px;
}

.input-icons {
  display: flex;
  align-items: center;
}

.input-icons .btn {
  background: none;
  border: none;
  cursor: pointer;
}

.btn {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px;
  font-size: 14px;
  font-weight: 600;
  color: #ffffff;
  background-color: #5c6bc0;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.2s ease;
}

.btn-icon {
  margin-right: 8px;
}

.btn:hover {
  background-color: #3949ab;
}

.btn:active {
  transform: scale(0.95);
  background-color: #303f9f;
}

.btn-secondary {
  background-color: #26a69a;
}

.btn-secondary:hover {
  background-color: #1b8779;
}

.btn-secondary:active {
  background-color: #177466;
}

.btn-trace {
  background-color: #ff7043;
}

.btn-trace:hover {
  background-color: #ff5722;
}

.btn-trace:active {
  background-color: #e64a19;
}

.btn:disabled {
  background-color: #bdbdbd;
  cursor: not-allowed;
}

.button-group {
  display: flex;
  justify-content: space-between;
  gap: 10px;
  margin-top: 10px;
}

.btn-send {
  padding: 10px;
  border-radius: 50%;
  background-color: #5c6bc0;
}

.btn-send:hover {
  background-color: #3949ab;
}

.btn-send:active {
  background-color: #303f9f;
  transform: scale(0.95);
}
