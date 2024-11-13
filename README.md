.questionBlock {
  display: grid;
  grid-template-areas:
    "question question"
    "leftSection rightSection";
  grid-template-columns: 1fr 1fr;
  grid-template-rows: auto 1fr; /* Allow rightSection to expand vertically */
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

/* Make rightSection (Citizen Speak) span the height */
.rightSection {
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

    {/* Citizen Experience (expanded to cover the bottom space) */}
    <div className={`${styles.responseSection} ${styles.rightSection}`}>
      <p><strong>Citizen Speak:</strong></p>
      {renderContent(answerData.citizenReview, "Citizen Experience")}
    </div>

    {/* Modal for Image */}
    <Modal isOpen={isModalOpen} closeModal={closeModal} imageSrc={imageSrc} altText="Modal Image" />
  </div>
);
