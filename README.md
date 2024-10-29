
/* Default Light Theme */
:root {
  --primary-color: #5f1ec1;
  --secondary-color: rgba(15, 95, 220, 1);
  --background-color: #ffffff;
  --inp-text-color: #00000;
  --chat-text-color:#fff;
  --scrollbar-color: rgba(15, 95, 220, 1);
  --scrollbar-background: #dcdcdc;
  --button-background-color: rgba(13, 85, 198, 0.1);
  --button-hover-color: #5f1ec1;
    --placeholder-color: #a9a9a9; /* Light theme placeholder color */
    --icon-color: #000000;

}

/* Dark Theme */
[data-theme="dark"] {
  --primary-color: #9d66f5;
  --secondary-color: #c1a1f2;
  --background-color: #1a1a2e;
  --inp-text-color: #ffffff;
    --text-color:#ffffff;
  --scrollbar-color: #5f1ec1;
  --scrollbar-background: #333333;
  --button-background-color: rgba(95, 30, 193, 0.8);
  --button-hover-color: #c1a1f2;
    --placeholder-color: #555555; /* Dark theme placeholder color */
        --icon-color: #ffffff;


}

/* Chatbot Styles */
.chatbotIcon {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background: linear-gradient(90deg, var(--primary-color) 0%, var(--secondary-color) 100%);
  color: var(--chat-text-color);
  padding: 10px;
  border-radius: 50%;
  cursor: pointer;
  z-index: 1000;
  animation: float 3s ease-in-out infinite;
}

@keyframes float {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-10px);
  }
}

.chatbotContainer {
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
  z-index: 1001;
  opacity: 0;
  transform: scale(0.9);
  transition: opacity 0.3s ease, transform 0.3s ease;
}

.chatbotContainer.open {
  opacity: 1;
  transform: scale(1);
}

.chatbotHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: linear-gradient(90deg, var(--primary-color) 0%, var(--secondary-color) 100%);
  color: var(--chat-text-color);
  padding: 10px;
  border-top-left-radius: 8px;
  border-top-right-radius: 8px;
}

.chatbotTitle {
  font-size: 16px;
  font-weight: bold;
}

.closeButton, .clearChatButton, .minimizeButton {
  background: none;
  border: none;
  color: var(--chat-text-color);
  font-size: 16px;
  cursor: pointer;
  margin-left: 5px;
}

.chatbotMessages {
  flex: 1;
  padding: 10px;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  animation: slideIn 0.5s ease;
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.userMessage, .botMessage {
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
  color: var(--chat-text-color)
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

.messageText a {
  color: var(--primary-color);
  text-decoration: none;
  font-size: 10px;
  margin-bottom: 5px;
}

.userMessage .messageText {
  background-color: #d1e7ff;
}

.chatbotInput {
  display: flex;
  border-top: 1px solid #ddd;
  padding: 10px;
}

.chatbotInput input::placeholder {
  color: var(--placeholder-color);
  /*font-style: italic;*/
}



.chatbotInput input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  color: var(--inp-text-color);
  outline: none;
}

.chatbotInput button {
  background: linear-gradient(90deg, var(--primary-color) 0%, var(--secondary-color) 100%);
  color: var(--chat-text-color);
  border: none;
  padding: 10px;
  border-radius: 4px;
  margin-left: 10px;
  cursor: pointer;
}

.clearChatOverlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1002;
}

.clearChatWindow {
  position: absolute;
  bottom: 130px; /* Adjust based on available space */
  left: 0;
  right: 0;
  background: var(--background-color);
  color: var(--text-color);
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.3);
  text-align: center;
}

.confirmButton {
  background-color: #d9534f;
  color: var(--chat-text-color);
  padding: 8px 18px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 10px;
}

.cancelButton {
  background-color: var(--primary-color);
  color: var(--chat-text-color);
  padding: 8px 18px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.botProfile {
  display: flex;
  align-items: center;
}

.botImage {
  width: 35px;
  height: 35px;
  border-radius: 50%;
  margin-right: 10px;
}

.botInfo {
  display: flex;
  flex-direction: column;
}

.botStatus {
  font-size: 12px;
  color: var(--chat-text-color);
  display: flex;
  align-items: center;
}

.onlineDot {
  width: 8px;
  height: 8px;
  background-color: #4caf50; /* Green dot */
  border-radius: 50%;
  display: inline-block;
  margin-right: 5px;
}
