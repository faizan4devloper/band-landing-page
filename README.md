.verifyContainer {
  padding: 20px;
}

.leftRightContainer {
  display: flex;
  justify-content: space-between;
  gap: 20px;
  padding-bottom: 20px; /* Add space between the panels and the button */
}

.leftPanel,
.rightPanel {
  width: 48%; /* Each panel takes up 48% of the screen */
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
  justify-content: center; /* This will center the button */
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
