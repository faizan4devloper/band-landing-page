/* MainContent.module.css */
/* Design System Variables */
:root {
  --primary-color: #2c3e50;
  --secondary-color: #3498db;
  --background-light: #f8f9fa;
  --border-color: #ecf0f1;
  --text-dark: #2c3e50;
  --text-light: #ffffff;
  --spacing-unit: 1rem;
  --border-radius: 8px;
  --box-shadow: 0 2px 15px rgba(0, 0, 0, 0.1);
}

.mainContentWrapper {
  padding: calc(var(--spacing-unit) * 2);
  height: 100vh;
  overflow-y: auto;
}

.claimIdDisplay {
  background-color: var(--primary-color);
  color: var(--text-light);
  padding: var(--spacing-unit);
  border-radius: var(--border-radius);
  margin-bottom: var(--spacing-unit);
  box-shadow: var(--box-shadow);
}

.mainContentGrid {
  display: grid;
  grid-template-columns: 1fr 1.5fr;
  gap: calc(var(--spacing-unit) * 2);
  height: calc(100% - 60px);
}

.leftColumn, .rightColumn {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-unit);
}

/* Card Layout Template */
.cardContainer {
  background: var(--text-light);
  border-radius: var(--border-radius);
  border: 1px solid var(--border-color);
  box-shadow: var(--box-shadow);
  padding: var(--spacing-unit);
  transition: transform 0.2s ease;
}

.cardContainer:hover {
  transform: translateY(-2px);
}

/* Specific Component Containers */
.claimClassificationContainer {
  flex: 0 1 auto;
}

.documentPreviewContainer {
  flex: 1 1 50%;
  min-height: 400px;
}

.percentageSection {
  height: 120px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.extractedContentContainer {
  flex: 1;
  min-height: 400px;
}

/* Verify Button Styling */
.verifyButtonContainer {
  margin-top: auto;
  padding-top: var(--spacing-unit);
}

.verifyButton {
  background-color: var(--secondary-color);
  color: var(--text-light);
  border: none;
  padding: calc(var(--spacing-unit) * 0.75) calc(var(--spacing-unit) * 2);
  border-radius: var(--border-radius);
  cursor: pointer;
  width: 100%;
  font-size: 1.1rem;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: var(--spacing-unit);
  transition: all 0.3s ease;
}

.verifyButton:hover {
  background-color: #2980b9;
  transform: scale(1.02);
}

.verifyButton:disabled {
  background-color: #95a5a6;
  cursor: not-allowed;
  opacity: 0.7;
}

/* Loading States */
.loadingText {
  color: var(--secondary-color);
  display: flex;
  align-items: center;
  gap: var(--spacing-unit);
}

/* Responsive Design */
@media (max-width: 1200px) {
  .mainContentGrid {
    grid-template-columns: 1fr;
    height: auto;
  }

  .leftColumn, .rightColumn {
    min-height: auto;
  }

  .documentPreviewContainer {
    min-height: 300px;
  }
}




// In your JSX
<div className={`${styles.claimClassificationContainer} ${styles.cardContainer}`}>
  <ClaimClassification ... />
</div>

<div className={`${styles.documentPreviewContainer} ${styles.cardContainer}`}>
  <DocumentPreview ... />
</div>

<div className={`${styles.percentageSection} ${styles.cardContainer}`}>
  {/* percentage content */}
</div>



{isLoading && (
  <div className={styles.loadingText}>
    <FontAwesomeIcon icon={faSync} spin />
    Loading...
  </div>
)}
