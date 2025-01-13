<div class="chat-container">
  <div class="message-area">
    <!-- User Message -->
    <div class="user-message">
      <p>Hello! How can I enable trace?</p>
    </div>
    <!-- Bot Response -->
    <div class="bot-response">
      <p>To enable trace, click the "Enable Trace" button.</p>
    </div>
  </div>
  
  <div class="input-area">
    <input type="text" placeholder="Type your message..." class="chat-input">
    <button class="btn btn-send">
      <i class="fa fa-paper-plane"></i>
    </button>
  </div>
  
  <div class="button-group">
    <button class="btn btn-secondary">Upload</button>
    <button class="btn btn-trace">Enable Trace</button>
  </div>
</div>





/* Chat Container */
.chat-container {
  display: flex;
  flex-direction: column;
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

/* Message Area */
.message-area {
  display: flex;
  flex-direction: column;
  gap: 15px;
  margin-bottom: 20px;
}

.user-message, .bot-response {
  max-width: 70%;
  padding: 10px 15px;
  border-radius: 10px;
}

.user-message {
  align-self: flex-start;
  background-color: #e0f7fa;
  color: #006064;
}

.bot-response {
  align-self: flex-end;
  background-color: #eceff1;
  color: #37474f;
}

/* Input Area */
.input-area {
  display: flex;
  gap: 10px;
  align-items: center;
}

.chat-input {
  flex-grow: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 20px;
  font-size: 14px;
}

/* Send Button */
.btn-send {
  width: 40px;
  height: 40px;
  padding: 0;
  border-radius: 50%;
  background-color: #5c6bc0;
  display: flex;
  align-items: center;
  justify-content: center;
  border: none;
  transition: background-color 0.3s ease, transform 0.2s ease;
}

.btn-send i {
  color: white;
  font-size: 18px;
}

.btn-send:hover {
  background-color: #3949ab;
}

.btn-send:active {
  background-color: #303f9f;
  transform: scale(0.95);
}

/* Button Group */
.button-group {
  display: flex;
  justify-content: space-between;
  gap: 10px;
  margin-top: 10px;
}

/* General Button Styles */
.btn {
  padding: 10px 20px;
  font-size: 14px;
  font-weight: 600;
  color: #ffffff;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.2s ease;
}

.btn:hover {
  background-color: #3949ab;
}

.btn:active {
  transform: scale(0.95);
  background-color: #303f9f;
}

/* Secondary Button */
.btn-secondary {
  background-color: #26a69a;
}

.btn-secondary:hover {
  background-color: #1b8779;
}

.btn-secondary:active {
  background-color: #177466;
}

/* Trace Button */
.btn-trace {
  background-color: #ff7043;
}

.btn-trace:hover {
  background-color: #ff5722;
}

.btn-trace:active {
  background-color: #e64a19;
}
