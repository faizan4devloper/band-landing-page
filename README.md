return (
  <div className={styles.questionBlock}>
    <div className={styles.question}>{question}</div>

    {/* Textual Response */}
    <div className={styles.responseSection}>
      <p><strong>School Sources Insights:</strong></p>
      {renderResponsePoints(answerData.textualResponse)}
    </div>

    {/* Citizen Experience (Expands to cover remaining space) */}
    <div className={`${styles.responseSection} ${styles.flexibleSection}`}>
      <p><strong>Citizen Speak:</strong></p>
      {renderContent(answerData.citizenReview, "Citizen Experience")}
    </div>

    {/* Factual Info */}
    <div className={styles.responseSection}>
      <p><strong>Factual Content:</strong></p>
      {renderContent(answerData.factualData, "Factual Info")}
    </div>

    {/* Modal for Image */}
    <Modal isOpen={isModalOpen} closeModal={closeModal} imageSrc={imageSrc} altText="Modal Image" />
  </div>
);



.questionBlock {
  display: flex;
  flex-direction: column;
  gap: 20px;
  margin: 20px 0;
  padding: 15px;
  border-radius: 8px;
  height: 100%; /* Ensures it takes full height available */
}

.responseSection {
  padding: 15px;
  font-size: 0.8rem;
  background-color: #f9f9f9;
  border-left: 2px solid #0073e6;
  border-radius: 6px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  position: relative;
}

/* This class will make Citizen Speak section expand */
.flexibleSection {
  flex-grow: 1; /* Allows it to grow and take remaining space */
  overflow-y: auto; /* Enables scrolling if content overflows */
}
