.mainContent {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 100vh;
  width: 100%;
  max-width: 800px;
  margin: 0 auto;
  background-color: #f9f9f9;
  border-radius: 10px;
  box-shadow: 0px 4px 12px rgba(0, 0, 0, 0.1);
  padding: 20px;
}

.messages {
  flex: 1;
  overflow-y: auto;
  padding-right: 10px;
  margin-bottom: 20px;
  scrollbar-width: thin;
  scrollbar-color: #c1c1c1 transparent;
}

.messages::-webkit-scrollbar {
  width: 8px;
}

.messages::-webkit-scrollbar-thumb {
  background-color: #c1c1c1;
  border-radius: 4px;
}

.userMessage,
.botMessage {
  display: flex;
  align-items: flex-start;
  margin-bottom: 15px;
  max-width: 70%;
}

.userMessage {
  align-self: flex-end;
  background-color: #4a90e2;
  color: white;
  border-radius: 10px 10px 0 10px;
}

.botMessage {
  align-self: flex-start;
  background-color: #e0e0e0;
  color: #333;
  border-radius: 10px 10px 10px 0;
}

.messageText {
  padding: 10px 15px;
  font-size: 1rem;
}

.icon {
  margin-right: 10px;
  font-size: 1.2rem;
  color: #5f1ec1;
}

.inputContainer {
  display: flex;
  align-items: center;
  padding: 10px;
  border-top: 1px solid #ccc;
  background-color: #fff;
}

input {
  flex: 1;
  padding: 10px;
  font-size: 1rem;
  border-radius: 25px;
  border: 1px solid #ccc;
  outline: none;
  margin-right: 10px;
  background-color: #f1f1f1;
}

input:focus {
  border-color: #5f1ec1;
}

.sendButton {
  background-color: #5f1ec1;
  color: white;
  border: none;
  padding: 10px;
  border-radius: 50%;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background-color 0.3s ease;
}

.sendButton:hover {
  background-color: #4a1ba0;
}

.sendButton:disabled {
  background-color: #999;
  cursor: not-allowed;
}

.sendButton:disabled:hover {
  background-color: #999;
}
