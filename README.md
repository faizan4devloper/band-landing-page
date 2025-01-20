<div className="chat-wrapper">
  <div className="upload-panel">
    <input
      type="file"
      id="file-upload"
      onChange={(e) => handleUpload(e.target.files[0])}
      style={{ display: 'none' }}
    />
    <div className="upload-wrapper">
      <FontAwesomeIcon
        icon={faPlusCircle}
        className="upload-plus-icon"
        onClick={() => document.getElementById('file-upload').click()}
      />
      <p>Upload File</p>
    </div>
  </div>

  <div className="chat-container">
    <div className="chat-content">
      <div className="chat-messages-container">
        <div className="chat-header">
          <FontAwesomeIcon icon={faComments} className="header-icon" />
          Chat with AI Ninja
        </div>

        <div className="chat-messages">
          {/* Chat messages go here */}
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
              disabled={isLoading}
            >
              {isLoading ? (
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
    </div>
  </div>
</div>



        .chat-wrapper {
  display: flex;
  gap: 20px;
  padding: 20px;
  background: linear-gradient(135deg, #eceff1, #f5f5f5);
}

.upload-panel {
  width: 250px;
  background: #ffffff;
  border-radius: 16px;
  padding: 20px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 15px;
}

.upload-plus-icon {
  font-size: 36px;
  color: #5c6bc0;
  cursor: pointer;
  transition: color 0.3s ease;
}

.upload-plus-icon:hover {
  color: #3949ab;
}

.upload-wrapper p {
  font-size: 14px;
  color: #5c6bc0;
  text-align: center;
}

.chat-container {
  flex: 1;
  display: flex;
  flex-direction: column;
  background: #ffffff;
  border-radius: 16px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
  overflow: hidden;
}

.btn-trace.active {
  background-color: #e64a19;
}
        
        
        
        
        
        
        
        
        
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
    <input
      type="file"
      id="image-upload"
      onChange={(e) => handleUpload(e.target.files[0])}
      style={{ display: 'none' }}
    />
    <div className="input-icons">
      <FontAwesomeIcon 
        icon={faUpload} 
        className="upload-icon" 
        onClick={() => document.getElementById('image-upload').click()}
      />
    </div>
      <button onClick={sendMessage} className="btn btn-send">
        <FontAwesomeIcon icon={faPaperPlane} />
      </button>
  </div>
          <div className="button-group">
          
                      <button 
            onClick={toggleTraceData} 
            className="btn btn-trace" 
            disabled={isLoading}
          >
            {isLoading ? (
              <BeatLoader color="#fff" size={12} />
            ) : showTracePanel ? (
              <>
                Hide Trace 
                <FontAwesomeIcon 
                  icon={faChevronRight} 
                  className="btn-icon right-chevron" 
                />
              </>
            ) : (
              <>
                Enable Trace 
                <FontAwesomeIcon 
                  icon={faChevronRight} 
                  className="btn-icon right-chevron" 
                />
              </>
            )}
          </button>

          </div>
        </div>


.chat-wrapper {
  display: flex;
  min-height: 100vh;
  background-color: #f5f5f5;
  gap: 20px;
  padding: 20px;
}

.chatbot-panel,
.trace-panel {
  background: #ffffff;
  border-radius: 16px;
  padding: 20px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
}

.chatbot-panel {
  /*flex: 3;*/
  width: 600px;
  height: 90vh;
}

.trace-panel {
  flex: 2;
  height: 90vh;
}

/*.chat-header {*/
/*  font-size: 20px;*/
/*  font-weight: bold;*/
/*  margin-bottom: 15px;*/
/*  color: #1462dd;*/
/*}*/

.message {
  margin-bottom: 10px;
}

.bot-message .message-text {
  background: #e3f2fd;
}

.user-message .message-text {
  background: #fbfbfb;
}

.btn {
  background-color: #5c6bc0;
  color: white;
  margin: 5px 0;
}


.chat-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  padding: 20px;
  background: linear-gradient(135deg, #eceff1, #f5f5f5);
}

.chat-container {
  width: 100%;
  max-width: 960px;
  background: #ffffff;
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
  background: linear-gradient(304deg, #499dfd 3.03%, #2678f5 27.62%, #6628c5 76.39%, #5b0bb1 112.44%);
  color: #ffffff;
  padding: 15px;
  text-align: center;
  font-weight: 600;
  border-radius: 10px;
  margin-bottom: 15px;
  font-size: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
}

.header-icon {
  font-size: 24px;
  /*margin-right: 10px;*/
}

.powered-by {
  position: relative;
  font-style: italic;
  font-size: 10px;
  transition: all 0.3s ease;
}

/*.powered-by::after {*/
/*  content: '';*/
/*  position: absolute;*/
/*  bottom: -2px;*/
/*  left: 0;*/
/*  width: 0;*/
/*  height: 2px;*/
/*  background: linear-gradient(to right, #ff9966, #ff5e62);*/
/*  transition: width 0.3s ease;*/
/*}*/

.powered-by:hover::after {
  width: 100%;
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
  animation: fadeIn 0.3s ease-in-out;
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
  font-size: 14px;
  max-width: 70%;
  word-wrap: break-word;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.user-message .message-text {
  background-color: #fbfbfb;
}

.bot-message .message-text {
  background-color: #bbdefb;
}

.chat-input-container {
  margin-top: 10px;
  display: flex;
  flex-direction: column;
}

.input-wrapper {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background-color: #ffffff;
  padding: 8px;
  border-radius: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.chat-input {
  flex: 1;
  padding: 10px;
  border-radius: 8px;
  border: 1px solid #e0e0e0;
  margin-right: 10px;
  font-size: 16px;
}

.input-icons .btn {
  background: none;
  border: none;
  cursor: pointer;
  padding: 8px;
}

.input-icons .btn-send {
  background:
linear-gradient(304deg,#499dfd 3.03%,#2678f5 27.62%,#6628c5 76.39%,#5b0bb1 112.44%)
;
  color: #ffffff;
  border-radius: 50%;
  padding: 12px;
  font-size: 18px;
  transition: background-color 0.3s ease;
}

.input-icons .btn-send:hover {
  background-color: #3949ab;
}

.button-group {
  display: flex;
  justify-content: space-between;
  margin-top: 10px;
}

.btn {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px 20px;
  font-size: 14px;
  font-weight: 600;
  color: #ffffff;
  background:
linear-gradient(304deg,#499dfd 3.03%,#2678f5 27.62%,#6628c5 76.39%,#5b0bb1 112.44%)
;
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

.trace-panel {
  width: 300px;
  padding: 20px;
  /*background-color: #5c6bc0;*/
  color: #000;
  overflow-y: auto;
  border-left: 1px solid #e0e0e0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
}

.trace-header h3 {
  font-size: 18px;
  margin-bottom: 10px;
}

.trace-content {
  width: 100%;
}

.trace-step {
  margin-bottom: 15px;
  background-color: #9fa8da;
  padding: 15px;
  border-radius: 8px;
  color: #ffffff;
}

.trace-step-header {
  font-weight: bold;
  margin-bottom: 5px;
}

.no-trace {
  text-align: center;
  color: #ddd;
}
.error-trace{
    text-align: center;


}

.input-wrapper {
  display: flex;
  align-items: center;
  position: relative;
  width: 100%;
}

.chat-input {
  flex-grow: 1;
  padding-right: 70px; /* Make space for icons */
}

.input-icons {
  position: absolute;
  left: 10px;
  display: flex;
  align-items: center;
  gap: 10px;
}

.upload-icon {
  cursor: pointer;
  color: #785ce5;
  margin-right: 10px;
  transition: color 0.3s ease;
}

.upload-icon:hover {
  color: #5c3a9c;
}





@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}


.typing-indicator {
  display: flex;
  justify-content: flex-end;
  padding: 10px;
}

.btn-send {
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #5c6bc0;
  border: none;
  color: white;
  padding: 10px;
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn-send:disabled {
  background-color: #cccccc;
  cursor: not-allowed;
}
.right-chevron{
  padding-left: 5px;
}
.btn-trace {
  background-color: #ff7043;
  transition: all 0.3s ease;
}

.btn-trace.active {
  background-color: #e64a19;
}
