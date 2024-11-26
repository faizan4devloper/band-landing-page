/* Main container layout */
.mainContent {
  display: grid;
  grid-template-columns: 1fr 1fr; /* Two equal-width columns */
  grid-template-rows: auto 60px; /* Rows: auto for content, fixed height for the button */
  gap: 20px; /* Space between grid items */
  padding: 20px; /* Space around the entire container */
  align-items: start; /* Aligns sections to the top */
  background-color: #f5f5f5; /* Background for the main container */
}

/* Left Section: Document Preview */
.previewSection {
  background: #ffffff; /* White background for better contrast */
  padding: 15px;
  border: 1px solid #ddd; /* Subtle border */
  border-radius: 8px;
}

.documentList {
  list-style: none; /* Remove default list styling */
  padding: 0;
  margin: 0;
}

.documentList li {
  margin-bottom: 10px;
}

.previewButton {
  background-color: #007bff; /* Blue background */
  color: white;
  padding: 10px 15px; /* Inner spacing */
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 14px; /* Adjusted font size for buttons */
  transition: background-color 0.3s ease; /* Smooth hover transition */
}

.previewButton:hover {
  background-color: #0056b3; /* Darker blue on hover */
}

/* Right Section: Extracted Content */
.extractContentSection {
  background: #ffffff; /* White background for better contrast */
  padding: 15px;
  border: 1px solid #ddd; /* Subtle border */
  border-radius: 8px;
}

/* "Verify" Button Container */
.verifyButtonContainer {
  grid-column: span 2; /* Span across both columns in the grid */
  display: flex;
  justify-content: center; /* Horizontally center the button */
  align-items: center; /* Vertically center the button */
  margin-top: 10px; /* Space above the button */
}

/* "Verify" Button Styling */
.verifyButton {
  padding: 12px 24px; /* Spacing around text */
  font-size: 16px; /* Larger font size for visibility */
  font-weight: bold; /* Make text bold */
  background-color: #28a745; /* Green background */
  color: white; /* White text */
  border: none;
  border-radius: 8px; /* Rounded corners */
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.2s ease; /* Smooth transition for hover effects */
}

.verifyButton:hover {
  background-color: #218838; /* Darker green on hover */
  transform: scale(1.05); /* Slight zoom effect on hover */
}

.verifyButton:active {
  background-color: #1e7e34; /* Even darker green when clicked */
  transform: scale(0.95); /* Shrink effect on click */
}

/* Modal Styling */
.modal {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: #ffffff;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
  width: 80%;
  max-width: 800px;
}

.modalOverlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  z-index: 10;
}

.modalContent {
  position: relative;
}

.closeButton {
  position: absolute;
  top: 10px;
  right: 10px;
  background: #ff4d4d;
  color: white;
  border: none;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  font-size: 16px;
}

.closeButton:hover {
  background: #cc0000;
}

/* Additional General Styles */
.readableContent {
  background: #f8f8f8;
  padding: 15px;
  border-radius: 8px;
  line-height: 1.6;
}

.readableContent h4 {
  color: #333;
  margin-bottom: 10px;
  text-decoration: underline;
}

.readableContent p {
  margin: 5px 0;
  color: #555;
}

.readableContent strong {
  color: #000;
}

h5 {
  margin-top: 15px;
  font-size: 1.1rem;
  color: #444;
}



/* Main container layout */
.mainContent {
  display: grid;
  grid-template-columns: 1fr 1fr; /* Two equal-width columns */
  grid-template-rows: auto 60px; /* Rows: auto for content, fixed height for the button */
  gap: 20px; /* Space between grid items */
  padding: 20px; /* Space around the entire container */
  align-items: start; /* Aligns sections to the top */
  background-color: #f5f5f5; /* Background for the main container */
}

/* Left Section: Document Preview */
.previewSection {
  background: #ffffff; /* White background for better contrast */
  padding: 15px;
  border: 1px solid #ddd; /* Subtle border */
  border-radius: 8px;
}

.documentList {
  list-style: none; /* Remove default list styling */
  padding: 0;
  margin: 0;
}

.documentList li {
  margin-bottom: 10px;
}

.previewButton {
  background-color: #007bff; /* Blue background */
  color: white;
  padding: 10px 15px; /* Inner spacing */
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 14px; /* Adjusted font size for buttons */
  transition: background-color 0.3s ease; /* Smooth hover transition */
}

.previewButton:hover {
  background-color: #0056b3; /* Darker blue on hover */
}

/* Right Section: Extracted Content */
.extractContentSection {
  background: #ffffff; /* White background for better contrast */
  padding: 15px;
  border: 1px solid #ddd; /* Subtle border */
  border-radius: 8px;
}

/* "Verify" Button Container */
.verifyButtonContainer {
  grid-column: span 2; /* Span across both columns in the grid */
  display: flex;
  justify-content: center; /* Horizontally center the button */
  align-items: center; /* Vertically center the button */
  margin-top: 10px; /* Space above the button */
}

/* "Verify" Button Styling */
.verifyButton {
  padding: 12px 24px; /* Spacing around text */
  font-size: 16px; /* Larger font size for visibility */
  font-weight: bold; /* Make text bold */
  background-color: #28a745; /* Green background */
  color: white; /* White text */
  border: none;
  border-radius: 8px; /* Rounded corners */
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.2s ease; /* Smooth transition for hover effects */
}

.verifyButton:hover {
  background-color: #218838; /* Darker green on hover */
  transform: scale(1.05); /* Slight zoom effect on hover */
}

.verifyButton:active {
  background-color: #1e7e34; /* Even darker green when clicked */
  transform: scale(0.95); /* Shrink effect on click */
}

/* Modal Styling */
.modal {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: #ffffff;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
  width: 80%;
  max-width: 800px;
}

.modalOverlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  z-index: 10;
}

.modalContent {
  position: relative;
}

.closeButton {
  position: absolute;
  top: 10px;
  right: 10px;
  background: #ff4d4d;
  color: white;
  border: none;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  font-size: 16px;
}

.closeButton:hover {
  background: #cc0000;
}

/* Additional General Styles */
.readableContent {
  background: #f8f8f8;
  padding: 15px;
  border-radius: 8px;
  line-height: 1.6;
}

.readableContent h4 {
  color: #333;
  margin-bottom: 10px;
  text-decoration: underline;
}

.readableContent p {
  margin: 5px 0;
  color: #555;
}

.readableContent strong {
  color: #000;
}

h5 {
  margin-top: 15px;
  font-size: 1.1rem;
  color: #444;
}
