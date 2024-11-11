
/* Styling for Follow-up Questions */
.followUpQuestion {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
  background-color: #e6f7ff;
  padding: 10px;
  border-radius: 8px;
  font-size: 14px;
  color: #333;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease;
}

/* Highlighting Follow-up Question on Hover */
.followUpQuestion:hover {
  background-color: #cceeff;
  transform: scale(1.05);
}

.followUpQuestion .icon {
  margin-right: 10px;
  font-size: 18px;
  color: var(--primary-color);
}

/* Styling for the Follow-up Question Text */
.followUpQuestion .questionText {
  flex: 1;
  word-wrap: break-word;
  white-space: pre-wrap;
}

/* Add these styles to the messages section */
.messages {
  flex: 1;
  padding: 10px;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
}

.messages .followUpQuestion {
  margin-top: 5px;
}

/* Optional: Add a small "Reply" icon beside follow-up questions */
.followUpQuestion .replyIcon {
  font-size: 14px;
  color: var(--primary-color);
  margin-left: 8px;
}




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
    color:#fff;
    /*padding: 14px;*/
    /*border-radius: 30%;*/
    /*background: linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%);*/
    right: 20px;
    z-index: 1000;
}

.iconContainer {
  cursor: pointer;  
  padding: 14px;
  border-radius: 30%;
  background: linear-gradient(90deg, var(--primary-color) 0%, var(--secondary-color) 100%);
  transition: transform 0.3s ease;
}

/* Chat Window - Modal Style */
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
}

.chatHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: linear-gradient(90deg, var(--primary-color) 0%, var(--secondary-color) 100%);
  color: var(--chat-text-color);
  padding: 5px;
  border-top-left-radius: 8px;
  border-top-right-radius: 8px;
}

.chatHeading{
  font-size: 14px;
  font-weight: bold;
  text-align:center;
}


.closeButton {
  background: none;
  border: none;
  color: var(--chat-text-color);
  font-size: 16px;
  cursor: pointer;
}

.messages {
  flex: 1;
  padding: 10px;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
}

.userMessage,
.botMessage {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.userMessage {
  justify-content: flex-end;
}

.botMessage {
  justify-content: flex-start;
}

.icon {
  margin: 0 8px;
  font-size: 15px;
  color: #fff;
}

.messageText {
  max-width: 75%;
  background-color: #f1f1f1;
  padding: 10px;
  font-size: 12px;
  border-radius: 10px;
  color: #333;
  word-wrap: break-word;
  white-space: pre-wrap;
}

.userMessage .messageText {
  background-color: #d1e7ff;
}

.inputForm {
  display: flex;
  border-top: 1px solid #ddd;
  padding: 10px;
}

.inputField {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  color: #000;
  outline: none;
}

.submitButton {
  background: linear-gradient(90deg, var(--primary-color) 0%, var(--secondary-color) 100%);
  color: var(--chat-text-color);
  border: none;
  padding: 10px;
  border-radius: 4px;
  margin-left: 10px;
  cursor: pointer;
}


.messages::-webkit-scrollbar{
  width:6px;
}
.messages::-webkit-scrollbar-thumb{
background-color: rgba(15, 95, 220, 1);
border-radius: 10px
}
