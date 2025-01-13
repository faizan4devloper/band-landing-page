/* Button Styles */
.btn {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px 20px;
  font-size: 14px;
  font-weight: 600;
  color: #ffffff;
  background-color: #5c6bc0;
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

/* Secondary Button (e.g., Upload) */
.btn-secondary {
  background-color: #26a69a;
}

.btn-secondary:hover {
  background-color: #1b8779;
}

.btn-secondary:active {
  background-color: #177466;
}

/* Trace Button */
.btn-trace {
  background-color: #ff7043;
}

.btn-trace:hover {
  background-color: #ff5722;
}

.btn-trace:active {
  background-color: #e64a19;
}

/* Disabled Button */
.btn:disabled {
  background-color: #bdbdbd;
  cursor: not-allowed;
}

/* Button Group Alignment */
.button-group {
  display: flex;
  justify-content: space-between;
  gap: 10px;
  margin-top: 10px;
}

/* Input Button */
.btn-send {
  padding: 8px 16px;
  border-radius: 50%;
  background-color: #5c6bc0;
}

.btn-send:hover {
  background-color: #3949ab;
}

.btn-send:active {
  background-color: #303f9f;
  transform: scale(0.95);
}
