.questionBlock {
  display: grid;
  grid-template-columns: 1fr 1fr 2fr; /* Two fixed-width columns and one larger column */
  grid-template-areas:
    "question question question"
    "textualResponse citizenExperience factualInfo";
  gap: 20px;
  position: relative;
  margin: 20px 0;
  border-radius: 8px;
  padding: 15px;
}

.question {
  grid-area: question;
  font-size: 1.3rem;
  font-weight: bold;
  color: #2a2a2a;
  margin-bottom: 15px;
}

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

/* Define individual sections for layout */
.responseSection.textualResponse {
  grid-area: textualResponse;
}

.responseSection.citizenExperience {
  grid-area: citizenExperience;
}

.responseSection.factualInfo {
  grid-area: factualInfo;
  width: 100%; /* Expand to cover the remaining space */
}







<div className={styles.questionBlock}>
  <div className={styles.question}>{question}</div>

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
