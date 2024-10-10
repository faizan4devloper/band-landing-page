/* Default Light Theme without :root and variables */

/* Chatbot Styles */
.chatbotIcon {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background: linear-gradient(135deg, #F2F2F2 -20%, #7ca2e1);
  color: #000;
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
  background-color: #ffffff;
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
  background: linear-gradient(90deg, #5f1ec1 0%, #c1a1f2 100%);
  color: #fff;
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
  color: #fff;
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

.messageText a {
  color: #5f1ec1;
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
  color: #a9a9a9;
}

.chatbotInput input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  color: #000;
  outline: none;
}

.chatbotInput button {
  background: linear-gradient(90deg, #5f1ec1 0%, rgba(15, 95, 220, 1) 100%);
  color: #fff;
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
  bottom: 130px;
  left: 0;
  right: 0;
  background: #ffffff;
  color: #000;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.3);
  text-align: center;
}

.confirmButton {
  background-color: #d9534f;
  color: #fff;
  padding: 8px 18px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 10px;
}

.cancelButton {
  background-color: #5f1ec1;
  color: #fff;
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
  color: #fff;
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
