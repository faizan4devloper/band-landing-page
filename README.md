/* Container */
.percentHead {
  color: #2c3e50;
  font-size: 1.2rem;
  font-weight: 600;
  margin: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  text-transform: uppercase;
}

/* Progress Bar Container */
.percentageContainer {
  width: 100%;
  height: 35px;
  background-color: #e0e0e0;
  border-radius: 20px;
  overflow: hidden;
  margin-top: 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  position: relative;
  display: flex;
  align-items: center;
}

/* Progress Bar */
.splitPercentageBar {
  display: flex;
  width: 100%;
  height: 100%;
  position: relative;
  border-radius: 20px;
}

/* Sections */
.emptyKeysSection,
.filledKeysSection {
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 600;
  font-size: 14px;
  transition: width 0.6s ease-in-out;
  white-space: nowrap;
  min-width: 0; /* Prevent text overflow */
  color: white;
}

.emptyKeysSection {
  background: linear-gradient(90deg, #ff6b6b, #ff3b3b);
}

.filledKeysSection {
  background: linear-gradient(90deg, #4ecb71, #2fa558);
}

/* Percentage Text */
.emptyKeysSection span,
.filledKeysSection span {
  padding: 5px 10px;
  border-radius: 15px;
  background: rgba(0, 0, 0, 0.2);
}

/* Loading Spinner */
.loadingSpinner {
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 14px;
  font-weight: bold;
  color: #2c3e50;
  margin-top: 12px;
}

.loadingSpinner::after {
  content: "";
  width: 15px;
  height: 15px;
  margin-left: 8px;
  border: 2px solid #0f5fdc;
  border-top: 2px solid transparent;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

/* Spinner Animation */
@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
