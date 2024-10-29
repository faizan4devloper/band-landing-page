.chatContainer {
  display: flex;
  flex-direction: column;
  width: 400px; /* Adjust the width as needed */
  height: 600px; /* Adjust the height as needed */
  border: 1px solid #ddd;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  background: #fff;
  overflow: hidden;
}

.chatWindow {
  flex: 1;
  padding: 15px;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  gap: 10px; /* Space between messages */
}

.userMessage,
.botMessage {
  display: flex;
  align-items: flex-start;
  max-width: 80%;
  padding: 10px;
  border-radius: 20px;
  position: relative;
}

.userMessage {
  align-self: flex-end;
  background-color: #cce5ff; /* Light blue for user messages */
  margin-left: auto; /* Align user messages to the right */
}

.botMessage {
  align-self: flex-start;
  background-color: #f1f1f1; /* Light gray for bot messages */
}

.icon {
  margin-right: 10px;
  color: #5f1ec1; /* Customize icon color */
  font-size: 1.2em;
}

.messageText {
  word-wrap: break-word; /* Ensure long messages wrap properly */
}

.inputForm {
  display: flex;
  padding: 10px;
  border-top: 1px solid #ddd;
  background-color: #f7f7f7; /* Light gray background for input area */
}

.inputField {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 20px;
  margin-right: 10px;
  font-size: 1em;
  transition: border-color 0.3s;
}

.inputField:focus {
  border-color: #5f1ec1; /* Change border color on focus */
  outline: none; /* Remove default outline */
}

.submitButton {
  background-color: #5f1ec1; /* Primary button color */
  border: none;
  border-radius: 50%;
  padding: 10px;
  cursor: pointer;
  color: white;
  transition: background-color 0.3s;
}

.submitButton:hover {
  background-color: #4a1a90; /* Darken button on hover */
}

.submitButton:disabled {
  background-color: #ccc; /* Gray out button when loading */
  cursor: not-allowed; /* Change cursor style */
}

/* Add a typing indicator style */
.typingIndicator {
  display: flex;
  align-items: center;
  padding: 10px;
  color: #666; /* Color for typing indicator */
}
