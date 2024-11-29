/* Main Container Styling */
.container {
  display: flex;
  gap: 20px;
  padding: 20px;
  background: linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
  min-height: 100vh;
  font-family: Arial, sans-serif;
  color: #ffffff; /* Set text color for better contrast */
}

/* Left and Right Section Styling */
.leftSection,
.rightSection {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 20px;
  padding: 15px;
  border-radius: 10px;
  background-color: #ffffff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  color: #0f5fdc; /* Text color matching primary theme */
}

/* Individual Window Styling */
.sectionWindow {
  background: #f9f9f9;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  padding: 20px;
  transition: transform 0.2s ease, box-shadow 0.2s ease, height 0.3s ease;
  color: #333; /* Neutral text color for readability */
}

.sectionWindow:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 12px rgba(0, 0, 0, 0.2);
}

.sectionWindow h2 {
  margin-bottom: 10px;
  font-size: 1.4rem;
  color: #0f5fdc;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.1);
}

.sectionWindow p {
  color: #555;
  font-size: 0.9rem;
  line-height: 1.5;
}

/* Email Content Styling */
.emailContent {
  white-space: pre-wrap;
  background-color: #ffffff;
  border: 1px solid #0f5fdc;
  border-radius: 8px;
  padding: 15px;
  margin-top: 15px;
  color: #0f5fdc;
  font-family: 'Courier New', Courier, monospace;
  font-size: 0.95rem;
  line-height: 1.6;
  overflow-y: auto;
  max-height: 400px;
  box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.1);
}

.emailContent::-webkit-scrollbar {
  width: 8px;
}

.emailContent::-webkit-scrollbar-thumb {
  background: #7ca2e1;
  border-radius: 4px;
}

.emailContent::-webkit-scrollbar-thumb:hover {
  background: #0f5fdc;
}

/* Buttons */
.generateButton,
.submitButton {
  padding: 10px 15px;
  background-color: #0f5fdc;
  color: #ffffff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1rem;
  transition: background-color 0.2s ease, transform 0.2s ease;
}

.generateButton:hover,
.submitButton:hover {
  background-color: #7ca2e1;
  transform: translateY(-2px);
}

.generateButton:disabled,
.submitButton:disabled {
  background-color: #d0d0d0;
  cursor: not-allowed;
}

/* Claim ID Display */
.claimIdDisplay {
  margin: 0px 25px;
  font-size: 16px;
  margin-bottom: 0px;
  font-weight: bold;
  color: #ffffff;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.2);
}
