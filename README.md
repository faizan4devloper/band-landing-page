.verifyContainer {
  padding: 20px;
}

.claimIdContainer {
  background-color: #007bff; /* Blue background for the Claim ID section */
  color: white; /* White text color */
  padding: 15px;
  text-align: center;
  font-size: 24px; /* Large font size for the Claim ID */
  font-weight: bold;
  border-radius: 8px;
  margin-bottom: 20px; /* Space below the Claim ID section */
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1); /* Soft shadow for depth */
}

.leftRightContainer {
  display: flex;
  justify-content: space-between;
  gap: 20px;
  padding-bottom: 20px;
}

.leftPanel,
.rightPanel {
  width: 48%;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.leftPanel h3,
.rightPanel h3 {
  margin-top: 0;
  color: #333;
}

.leftPanel p,
.rightPanel p {
  font-size: 16px;
  line-height: 1.5;
  color: #555;
}

.generateEmailContainer {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.generateEmailButton {
  padding: 10px 20px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s;
}

.generateEmailButton:hover {
  background-color: #0056b3;
}

/* Styling for the loading spinner */
.spinnerContainer {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh; /* Make the spinner cover the full viewport */
}
