/* Default Light Theme */
:root {
  --chatbot-background: white;
  --chatbot-header-background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  --chatbot-message-user: #d1e7ff;
  --chatbot-message-bot: #f1f1f1;
  --chatbot-button-background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  --chatbot-text-color: #333;
  --chatbot-message-link: #3f1ec2;
  --clear-chat-overlay-background: rgba(0, 0, 0, 0.5);
  --clear-chat-window-background: white;
  --confirm-button-background: #d9534f;
  --cancel-button-background: #5f1ec1;
  --bot-status-color: #d1e7ff;
  --online-dot-color: #4caf50;
}

/* Dark Theme */
[data-theme="dark"] {
  --chatbot-background: #333;
  --chatbot-header-background: linear-gradient(90deg, #3b3b3b 0%, #1f1f1f 100%);
  --chatbot-message-user: #444;
  --chatbot-message-bot: #555;
  --chatbot-button-background: linear-gradient(90deg, #3b3b3b 0%, #1f1f1f 100%);
  --chatbot-text-color: #ccc;
  --chatbot-message-link: #9a9a9a;
  --clear-chat-overlay-background: rgba(0, 0, 0, 0.8);
  --clear-chat-window-background: #444;
  --confirm-button-background: #c62828;
  --cancel-button-background: #1a237e;
  --bot-status-color: #555;
  --online-dot-color: #4caf50;
}

.chatbotIcon {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background: var(--chatbot-button-background);
  color: var(--chatbot-text-color);
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
  background-color: var(--chatbot-background);
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
  background: var(--chatbot-header-background);
  color: var(--chatbot-text-color);
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
  color: var(--chatbot-text-color);
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
}

.messageText {
  max-width: 75%;
  background-color: var(--chatbot-message-bot);
  padding: 10px;
  font-size: 12px;
  border-radius: 10px;
  color: var(--chatbot-text-color);
  word-wrap: break-word;
  white-space: pre-wrap;
}

.messageText a {
  color: var(--chatbot-message-link);
  text-decoration: none;
  font-size: 10px;
  margin-bottom: 5px;
}

.userMessage .messageText {
  background-color: var(--chatbot-message-user);
}

.chatbotInput {
  display: flex;
  border-top: 1px solid #ddd;
  padding: 10px;
}

.chatbotInput input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  outline: none;
}

.chatbotInput button {
  background: var(--chatbot-button-background);
  color: var(--chatbot-text-color);
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
  background: var(--clear-chat-overlay-background);
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
  background: var(--clear-chat-window-background);
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.3);
  text-align: center;
}

.confirmButton {
  background-color: var(--confirm-button-background);
  color: white;
  padding: 8px 18px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 10px;
}

.cancelButton {
  background-color: var(--cancel-button-background);
  color: white;
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
  color: var(--bot-status-color);
  display: flex;
  align-items: center;
}

.onlineDot {
  width: 8px;
  height: 8px;
  background-color: var(--online-dot-color);
  border-radius: 50%;
  display: inline-block;
  margin-right: 5px;
}
