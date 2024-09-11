/* Base light theme */
:root {
  --light-primary-color: #5f1ec1;
  --light-background-color: #f9f9f9;
  --light-border-color: rgba(95, 30, 193, 0.8);
  --light-text-color: #000;
  --light-placeholder-color: #999999;
  --light-button-bg-color: #5f1ec1;
  --light-button-text-color: #fff;
  --light-input-border-color: #ccc;
  --light-input-focus-border-color: #5f1ec1;
}

/* Dark theme overrides */
[data-theme="dark"] {
  --dark-primary-color: #d6bcfa;
  --dark-background-color: #1a1a1a;
  --dark-border-color: rgba(214, 188, 250, 0.8);
  --dark-text-color: #fff;
  --dark-placeholder-color: #777;
  --dark-button-bg-color: #d6bcfa;
  --dark-button-text-color: #1a1a1a;
  --dark-input-border-color: #444;
  --dark-input-focus-border-color: #d6bcfa;
}
.formContainer {
  padding: 30px;
  background-color: var(--light-background-color);
  background-color: var(--dark-background-color); /* Fallback for dark */
  border-left: 4px solid var(--light-border-color);
  border-left: 4px solid var(--dark-border-color); /* Fallback for dark */
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
  color: var(--light-primary-color);
  color: var(--dark-primary-color); /* Fallback for dark */
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
  color: var(--light-text-color);
  color: var(--dark-text-color); /* Fallback for dark */
  font-size: 12px;
  transition: color 0.3s;
}

input::placeholder, textarea::placeholder {
  color: var(--light-placeholder-color); /* Light theme placeholder */
  color: var(--dark-placeholder-color); /* Dark theme placeholder */
  opacity: 1;
}

input, textarea {
  width: 90%;
  padding: 5px;
  border: none;
  background-color: transparent;
  border-bottom: 1px solid var(--light-input-border-color); /* Light theme */
  border-bottom: 1px solid var(--dark-input-border-color); /* Dark theme */
  border-radius: 0;
  font-size: 12px;
  transition: border-color 0.3s;
  font-family: "Poppins", sans-serif; /* Apply Google Font */
}

input:focus, textarea:focus {
  border-color: var(--light-input-focus-border-color); /* Light theme */
  border-color: var(--dark-input-focus-border-color); /* Dark theme */
  outline: none;
}

.submitButton {
  background-color: var(--light-button-bg-color); /* Light theme */
  background-color: var(--dark-button-bg-color); /* Dark theme */
  margin-top: 0px;
  color: var(--light-button-text-color); /* Light theme */
  color: var(--dark-button-text-color); /* Dark theme */
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  margin-top: 10px;
  width: 78%;
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
  color: var(--light-primary-color); /* Light theme */
  color: var(--dark-primary-color); /* Dark theme */
  font-size: 16px;
}
