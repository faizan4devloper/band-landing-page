/* General Container */
.container {
  display: flex;
  flex-wrap: wrap;
  gap: 30px;
  padding: 30px;
  background-color: #f7f8fa;
  min-height: 100vh;
  font-family: Arial, sans-serif;
  justify-content: space-between;
  align-items: flex-start;
}

/* Left and Right Section Styling */
.leftSection,
.rightSection {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 20px;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.1);
  border: 1px solid #ddd;
  background: white;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.leftSection:hover,
.rightSection:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.15);
}

/* Individual Section Windows */
.sectionWindow {
  background: #ffffff;
  border-radius: 10px;
  padding: 20px;
  transition: all 0.3s ease-in-out;
}

.sectionWindow:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
}

.sectionWindow h2 {
  font-size: 1.5rem;
  margin-bottom: 15px;
  color: #333;
  font-weight: bold;
}

.sectionWindow p {
  color: #666;
  font-size: 1rem;
  line-height: 1.5;
}

/* Buttons */
.generateButton,
.submitButton {
  padding: 12px 20px;
  font-size: 1rem;
  border-radius: 5px;
  border: none;
  cursor: pointer;
  font-weight: bold;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
}

.generateButton {
  background-color: #2196f3;
  color: white;
}

.generateButton:hover {
  background-color: #1976d2;
}

.submitButton {
  background-color: #4caf50;
  color: white;
}

.submitButton:hover {
  background-color: #388e3c;
}

/* Claim ID Display */
.claimIdDisplay {
  text-align: center;
  font-size: 1.2rem;
  margin-bottom: 20px;
  font-weight: bold;
  color: #333;
}

/* Email Content Styling */
.emailContent {
  padding: 15px;
  background: #f9f9f9;
  border-radius: 8px;
  border: 1px solid #ddd;
  font-family: 'Courier New', monospace;
  font-size: 0.9rem;
  color: #444;
  line-height: 1.6;
  overflow-y: auto;
  max-height: 300px;
  white-space: pre-wrap;
  box-shadow: inset 0 3px 6px rgba(0, 0, 0, 0.1);
}

/* Error Text */
.errorText {
  color: #e53935;
  font-size: 1rem;
  font-weight: bold;
}

/* Responsive Design */
@media (max-width: 768px) {
  .container {
    flex-direction: column;
    padding: 15px;
  }

  .leftSection,
  .rightSection {
    flex: none;
    width: 100%;
  }
}
