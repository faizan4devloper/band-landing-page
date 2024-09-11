/* Base light theme */
:root {
  --primary-color: #5f1ec1;
  --background-color: #f9f9f9;
  --border-color: rgba(95, 30, 193, 0.8);
  --text-color: #000;
  --placeholder-color: #999999;
  --button-bg-color: #5f1ec1;
  --button-text-color: #fff;
  --input-border-color: #ccc;
  --input-focus-border-color: #5f1ec1;
}

/* Dark theme overrides */
[data-theme="dark"] {
  --primary-color: #d6bcfa;
  --background-color: #1a1a1a;
  --border-color: rgba(214, 188, 250, 0.8);
  --text-color: #fff;
  --placeholder-color: #777;
  --button-bg-color: #d6bcfa;
  --button-text-color: #1a1a1a;
  --input-border-color: #444;
  --input-focus-border-color: #d6bcfa;
}

.formContainer {
  padding: 30px;
  background-color: var(--background-color);
  border-left: 4px solid var(--border-color);
  margin-bottom: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  max-width: 450px;
  margin: 0 auto;
  animation: slideIn 0.5s ease-out;
  max-height: 80vh;
  z-index: 1000;
}

.demoHead {
  color: var(--primary-color);
  margin-bottom: 10px;
  text-align: center;
  margin-top: -10px;
  font-size: 18px;
}

.formGroup {
  margin-bottom: 10px;
  position: relative;
}

label {
  display: block;
  margin-bottom: 5px;
  font-weight: 600;
  color: var(--text-color);
  font-size: 12px;
  transition: color 0.3s;
}

input::placeholder, textarea::placeholder {
  color: var(--placeholder-color); /* Light gray */
  opacity: 1;
}

input, textarea {
  width: 90%;
  padding: 5px;
  border: none;
  background-color: transparent;
  border-bottom: 1px solid var(--input-border-color); /* Only bottom border */
  border-radius: 0;
  font-size: 12px;
  transition: border-color 0.3s;
  font-family: "Poppins", sans-serif; /* Apply Google Font */
}

input:focus, textarea:focus {
  border-color: var(--input-focus-border-color);
  outline: none;
}

.submitButton {
  background-color: var(--button-bg-color);
  margin-top: 0px;
  color: var(--button-text-color);
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  margin-top: 10px;
  width: 78%; /* Make the button take the full width */
}

.closeButton {
  position: absolute;
  top: 25px;
  right: 25px;
  background-color: transparent;
  border: none;
  cursor: pointer;
  font-size: 24px;
  color: #aaa;
}

.closeButton:hover {
  color: #555;
}

.successMessage {
  text-align: center;
  color: var(--primary-color);
  font-size: 16px;
}
