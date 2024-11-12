.chatContainer {
    position: fixed;
    bottom: 20px;
    color: #fff;
    right: 20px;
    z-index: 1000;
}

.iconContainer {
    cursor: pointer;
    padding: 14px;
    border-radius: 30%;
    background: linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%);
    transition: transform 0.3s ease;
}

.chatWindow {
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
    transition: all 0.3s ease;
}

.chatHeader {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background: linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%);
    color: #fff;
    padding: 5px;
    border-top-left-radius: 8px;
    border-top-right-radius: 8px;
}

.chatHeading {
    font-size: 14px;
    font-weight: bold;
    text-align: center;
}

.closeButton {
    background: none;
    border: none;
    color: #fff;
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
    background: linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%);
    color: #fff;
    border: none;
    padding: 10px;
    border-radius: 4px;
    margin-left: 10px;
    cursor: pointer;
}

.messages::-webkit-scrollbar {
    width: 6px;
}

.messages::-webkit-scrollbar-thumb {
    background-color: rgba(15, 95, 220, 1);
    border-radius: 10px;
}
