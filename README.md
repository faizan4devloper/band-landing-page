/* Light and Dark Theme Variables */
:root {
  --bg-color-light: #f9f9f9;
  --bg-color-dark: #2b2b2b;
  
  --text-color-light: #000;
  --text-color-dark: #fff;
  
  --border-color-light: rgba(95, 30, 193, 0.8);
  --border-color-dark: rgba(255, 255, 255, 0.8);
  
  --input-border-light: #ccc;
  --input-border-dark: #888;
  
  --input-focus-light: #5f1ec1;
  --input-focus-dark: #d4a1ff;
  
  --placeholder-light: #999;
  --placeholder-dark: #aaa;
  
  --button-bg-light: #5f1ec1;
  --button-bg-dark: #9d66ff;
  
  --scrollbar-bg-light: #f1f1f1;
  --scrollbar-bg-dark: #4a4a4a;
  
  --scrollbar-thumb-light: #5f1ec1;
  --scrollbar-thumb-dark: #d4a1ff;
}

/* Theme-specific styles */
[data-theme="light"] .formContainer {
  background-color: var(--bg-color-light);
  color: var(--text-color-light);
  border-left-color: var(--border-color-light);
}

[data-theme="dark"] .formContainer {
  background-color: var(--bg-color-dark);
  color: var(--text-color-dark);
  border-left-color: var(--border-color-dark);
}

[data-theme="light"] input, [data-theme="light"] textarea {
  border-bottom: 1px solid var(--input-border-light);
  color: var(--text-color-light);
}

[data-theme="dark"] input, [data-theme="dark"] textarea {
  border-bottom: 1px solid var(--input-border-dark);
  color: var(--text-color-dark);
}

input:focus, textarea:focus {
  border-color: var(--input-focus-light);
}

[data-theme="dark"] input:focus, [data-theme="dark"] textarea:focus {
  border-color: var(--input-focus-dark);
}

input::placeholder, textarea::placeholder {
  color: var(--placeholder-light);
}

[data-theme="dark"] input::placeholder, [data-theme="dark"] textarea::placeholder {
  color: var(--placeholder-dark);
}

.submitButton {
  background-color: var(--button-bg-light);
}

[data-theme="dark"] .submitButton {
  background-color: var(--button-bg-dark);
}

.closeButton {
  color: #aaa;
}

.closeButton:hover {
  color: #555;
}

/* Custom scrollbar styling for form container */
.formContainer::-webkit-scrollbar {
  width: 6px;
}

[data-theme="light"] .formContainer::-webkit-scrollbar-track {
  background: var(--scrollbar-bg-light);
}

[data-theme="dark"] .formContainer::-webkit-scrollbar-track {
  background: var(--scrollbar-bg-dark);
}

[data-theme="light"] .formContainer::-webkit-scrollbar-thumb {
  background: var(--scrollbar-thumb-light);
}

[data-theme="dark"] .formContainer::-webkit-scrollbar-thumb {
  background: var(--scrollbar-thumb-dark);
}

.formContainer::-webkit-scrollbar-thumb:hover {
  background: #555;
}
