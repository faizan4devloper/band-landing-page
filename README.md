.mainContentGrid {
  display: grid;
  grid-template-columns: 1.5fr 2fr;
  gap: 1.5rem;
  padding: 1.5rem;
  height: auto;
  align-items: flex-start;
}

.cardContainer {
  background: white;
  border-radius: 8px;
  padding: 1.5rem;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s ease;
}

.cardContainer:hover {
  transform: translateY(-3px);
}

/* Claim ID Display */
.claimIdDisplay {
  margin-bottom: 1rem;
  padding: 0.75rem 1.25rem;
  background: #f8f9fa;
  border-radius: 6px;
  font-weight: 600;
}

/* Column Layout */
.leftColumn, .rightColumn {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

/* Document Preview & Extracted Content */
.documentPreviewContainer {
  min-height: 320px;
  max-height: 500px;
  overflow: hidden;
  position: relative;
}

.extractedContentContainer {
  flex: 1;
  min-height: 400px;
  background: #f9f9f9;
}

/* Percentage Section */
.percentageSection {
  padding: 1.5rem;
  text-align: center;
  font-size: 1.1rem;
  font-weight: 600;
  border: 1px solid #ddd;
}

/* Verify Button */
.verifyButtonContainer {
  text-align: center;
}

.verifyButton {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 0.85rem 1.5rem;
  border-radius: 6px;
  cursor: pointer;
  font-size: 1.1rem;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.75rem;
  transition: all 0.3s ease;
  width: 100%;
}

.verifyButton:hover {
  background-color: #0056b3;
  transform: scale(1.03);
}

.verifyButton:disabled {
  background-color: #b0b8bf;
  cursor: not-allowed;
}

/* Responsive Design */
@media (max-width: 1024px) {
  .mainContentGrid {
    grid-template-columns: 1fr;
  }

  .documentPreviewContainer {
    min-height: 280px;
  }
}

@media (max-width: 768px) {
  .cardContainer {
    padding: 1rem;
  }

  .verifyButton {
    font-size: 1rem;
    padding: 0.75rem 1.25rem;
  }
}
