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
  max-width: 600px;
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
  font-size: 14px;
  transition: all 0.3s ease;
}

.powered-by::after {
  content: '';
  position: absolute;
  bottom: -2px;
  left: 0;
  width: 0;
  height: 2px;
  background: linear-gradient(to right, #ff9966, #ff5e62);
  transition: width 0.3s ease;
}

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
//////////////////////////////////////////////////















trace.css


:root {
  /* Color Palette */
  --primary-color: #785ce5;
  --secondary-color: #6a47c3;
  --background-light: #f5f5f5;
  --background-dark: #f4f4f4;
  --text-primary: #333;
  --text-secondary: #666;
  --border-color: #e0e0e0;
  
  /* Typography */
  --font-family-base: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  --font-family-mono: 'Courier New', Courier, monospace;
  
  /* Spacing */
  --space-xs: 0.5rem;
  --space-sm: 0.75rem;
  --space-md: 1rem;
  --space-lg: 1.5rem;
  
  /* Transitions */
  --transition-speed: 0.3s;
  
  /* Shadows */
  --shadow-subtle: 0 4px 15px rgba(0, 0, 0, 0.1);
}

.trace-panel {
  width: 100%;
  max-width: 600px;
  background-color: white;
  border-radius: 16px;
  box-shadow: var(--shadow-subtle);
  border: 1px solid var(--border-color);
  overflow: hidden;
  font-family: var(--font-family-base);
}

.trace-header {
     padding: 15px;
    background: linear-gradient(304deg, #499dfd 3.03%, #2678f5 27.62%, #6628c5 76.39%, #5b0bb1 112.44%);
    border-bottom: 1px solid var(--border-color);
    display: flex
;
    border-radius: 10px;
    color: #fff;
    align-items: center;
    justify-content: space-between;
    position: relative;
    overflow: hidden;
    font-weight: 600;
    font-size: 16px;
    margin-bottom: 10px;
}


/* Responsive Adjustments */
@media (max-width: 768px) {
  .trace-header {
    padding: var(--space-sm) var(--space-md);
  }
  
  .trace-header h3 {
    font-size: 1rem;
  }
}

.trace-content {
  max-height: 600px;
  overflow-y: auto;
  scrollbar-width: thin;
}

.trace-step-card {
  border-bottom: 1px solid var(--border-color);
  transition: background-color var(--transition-speed) ease;
   box-shadow: 
    0 1px 2px rgba(0, 0, 0, 0.04);
}


.trace-step-header {

  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: var(--space-md) var(--space-lg);
  cursor: pointer;
  transition: background-color var(--transition-speed) ease;
}

.trace-step-header:hover {
  background-color: var(--background-light);
}

.trace-step-header-content {
  display: flex;
  flex-direction: column;
  flex-grow: 1;
}

.trace-step-number {
  font-weight: 600;
  
  color: var(--primary-color);
  font-size: 1rem;
}

.trace-agent-name {
  font-size: 0.8rem;
  color: var(--text-secondary);
  margin-top: var(--space-xs);
}

.expand-icon {
  color: var(--primary-color);
  transition: transform var(--transition-speed) ease;
}

.trace-step-details {
  background-color: white;
  padding: var(--space-lg);
}

.trace-step-metadata {
  display: flex;
  justify-content: space-between;
  font-size: 14px;
  align-items: center;
  margin-bottom: var(--space-md);
  padding-bottom: var(--space-md);
  border-bottom: 1px solid var(--border-color);
}

.copy-btn {
      display: flex
;
    align-items: center;
    gap: 4px;
    /* background: linear-gradient(304deg, #499dfd 3.03%, #2678f5 27.62%, #6628c5 76.39%, #5b0bb1 112.44%); */
    color: #785ce5;
    border: none;
    padding: 8px 12px;
    border-radius: 8px;
    font-size: 0.8rem;
    cursor: pointer;
    transition: background-color var(--transition-speed) ease;
}

/*.copy-btn:hover {*/
/*  background-color: var(--secondary-color);*/
/*}*/

.copy-btn.copied {
  background-color: #4CAF50;
}

.json-viewer-container {
  background-color: var(--background-dark);
  border-radius: 12px;
  padding: var(--space-md);
  max-height: 220px;
  overflow-y: auto;
}

.custom-json-viewer {
  font-family: var(--font-family-mono);
  font-size: 0.8rem;
  line-height: 1.6;
}

/* JSON Syntax Highlighting */
.json-key { color: var(--primary-color); }
.json-string { color: #2ea44f; }
.json-number { color: #0366d6; }
.json-boolean { color: #d73a49; }
.json-null { 
  color: #6a737d; 
  font-style: italic; 
}

/* Responsive Design */
@media (max-width: 768px) {
  .trace-panel {
    max-width: 100%;
    border-radius: 0;
  }

  .trace-step-header,
  .trace-step-details {
    padding: var(--space-sm) var(--space-md);
  }

  .trace-step-metadata {
    flex-direction: column;
    align-items: flex-start;
    gap: var(--space-sm);
  }
}

/* Accessibility and Focus States */
.trace-step-header:focus-visible,
.copy-btn:focus-visible {
  outline: 2px solid var(--primary-color);
  outline-offset: 2px;
}

/* Animation */
@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

.trace-step-details {
  animation: fadeIn var(--transition-speed) ease-in-out;
}
