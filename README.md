.questionBlock {
  display: flex;
  flex-direction: column;
  gap: 20px;
  position: relative;
  margin: 20px 0;
  border-radius: 8px;
  padding: 15px;
}

.question {
  font-size: 1.3rem;
  font-weight: bold;
  color: #2a2a2a;
  margin-bottom: 15px;
}

/* Container for response sections */
.responseContainer {
  display: flex;
  gap: 20px;
}

/* Fixed-width for the left sections */
.responseSection {
  padding: 15px;
  font-size: 0.8rem;
  background-color: #f9f9f9;
  border-left: 2px solid #0073e6;
  border-radius: 6px;
  max-height: 200px;
  overflow-y: auto;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

/* Fixed-width for Textual Response and Citizen Experience */
.textualResponse,
.citizenExperience {
  flex: 0 0 200px; /* Set fixed width, e.g., 200px */
}

/* Expandable Factual Info */
.factualInfo {
  flex: 1; /* Fills remaining space */
  width: 100%;
}









<div className={styles.questionBlock}>
  <div className={styles.question}>{question}</div>

  {/* Container for response sections */}
  <div className={styles.responseContainer}>
    {/* Textual Response */}
    <div className={`${styles.responseSection} ${styles.textualResponse}`}>
      <p><strong>Textual Response:</strong></p>
      {renderResponsePoints(answerData.textualResponse)}
    </div>

    {/* Citizen Experience */}
    <div className={`${styles.responseSection} ${styles.citizenExperience}`}>
      <p><strong>Citizen Experience:</strong></p>
      {renderContent(answerData.citizenReview, "Citizen Experience")}
    </div>

    {/* Factual Info */}
    <div className={`${styles.responseSection} ${styles.factualInfo}`}>
      <p><strong>Factual Info:</strong></p>
      {renderContent(answerData.factualData, "Factual Info")}
    </div>
  </div>
</div>
