/* General styles */
.mainContentWrapper {
  display: flex;
  flex-direction: column;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  max-width: 1200px;
  margin: auto;
}

/* Claim ID styling */
.claimId {
  font-size: 18px;
  font-weight: 600;
  color: #333;
  margin-bottom: 15px;
}

/* Layout grid */
.mainGrid {
  display: grid;
  grid-template-columns: 1fr 1.5fr;
  gap: 20px;
}

/* Left column */
.leftColumn {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

/* Right column */
.rightColumn {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

/* Processing status */
.percentageSection {
  padding: 10px;
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
}

/* Extracted content container */
.extractedContentContainer {
  padding: 15px;
  background: #fff;
  border-radius: 10px;
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.1);
}

/* Verify button */
.verifyButtonContainer {
  display: flex;
  justify-content: flex-end;
  margin-top: 10px;
}

.verifyButton {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 10px 18px;
  font-size: 16px;
  font-weight: 500;
  border-radius: 8px;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 8px;
  transition: all 0.3s ease-in-out;
}

.verifyButton:hover {
  background-color: #0056b3;
  transform: translateY(-2px);
}

.verifyButton:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}
