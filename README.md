.questionBlock {
  display: grid;
  grid-template-areas:
    "question question"
    "leftSection rightSection";
  grid-template-columns: 1fr 1fr;
  grid-gap: 20px 40px;
  position: relative;
  margin: 20px 0;
  border-radius: 8px;
  padding: 15px;
}

.responseSection {
  padding: 15px;
  font-size: 0.8rem;
  background-color: #f9f9f9;
  border-left: 2px solid #0073e6;
  border-radius: 6px;
  position: relative;
  overflow-y: auto;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: max-height 0.4s ease, padding 0.4s ease;
}

/* Make Citizen Speak section grow to take available space */
.citizenSpeakSection {
  grid-area: rightSection;
  height: 100%;
}



return (
  <div className={styles.questionBlock}>
    <div className={styles.question}>{question}</div>

    {/* Textual Response */}
    <div className={styles.responseSection} style={{ gridArea: 'leftSection' }}>
      <p><strong>School Sources Insights:</strong></p>
      {renderResponsePoints(answerData.textualResponse)}
    </div>

    {/* Citizen Experience (Display as text or image) */}
    <div className={`${styles.responseSection} ${styles.citizenSpeakSection}`}>
      <p><strong>Citizen Speak:</strong></p>
      {renderContent(answerData.citizenReview, "Citizen Experience")}
    </div>

    {/* Modal for Image */}
    <Modal isOpen={isModalOpen} closeModal={closeModal} imageSrc={imageSrc} altText="Modal Image" />
  </div>
);












.questionBlock {
  display: grid;
  grid-template-columns: 1fr 1fr; /* Two columns of equal width */
  grid-template-rows: auto auto auto; /* Three rows for question, main sections, and bottom section */
  gap: 20px 40px;
  margin: 20px 0;
  padding: 15px;
  border-radius: 8px;
}

.question {
  grid-column: 1 / -1; /* Span across both columns */
  text-align: center;
  font-size: 1.3rem;
  font-weight: bold;
  color: #2a2a2a;
  margin-bottom: 15px;
}

.leftSection {
  grid-column: 1; /* First column */
}

.rightSection {
  grid-column: 2; /* Second column */
}

.bottomSection {
  grid-column: 1 / -1; /* Span across both columns */
}



return (
  <div className={styles.questionBlock}>
    <div className={styles.question}>{question}</div>

    {/* Left Section */}
    <div className={styles.leftSection}>
      <p><strong>School Sources Insights:</strong></p>
      {renderResponsePoints(answerData.textualResponse)}
    </div>

    {/* Right Section */}
    <div className={styles.rightSection}>
      <p><strong>Citizen Speak:</strong></p>
      {renderContent(answerData.citizenReview, "Citizen Experience")}
    </div>

    {/* Bottom Section */}
    <div className={styles.bottomSection}>
      <p><strong>Factual Content:</strong></p>
      {renderContent(answerData.factualData, "Factual Info")}
    </div>

    {/* Modal for Image */}
    <Modal isOpen={isModalOpen} closeModal={closeModal} imageSrc={imageSrc} altText="Modal Image" />
  </div>
);
