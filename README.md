return (
  <div className={styles.questionBlock}>
    <div className={styles.question}>{question}</div>

    {/* Textual Response */}
    <div className={styles.responseSection}>
      <p><strong>School Sources Insights:</strong></p>
      {renderResponsePoints(answerData.textualResponse)}
    </div>

    {/* Citizen Experience (Display as text or image) */}
    <div className={`${styles.responseSection} ${styles.flexibleSection}`}>
      <p><strong>Citizen Speak:</strong></p>
      {renderContent(answerData.citizenReview, "Citizen Experience")}
    </div>

    {/* Factual Info (Display as text or image) */}
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
  border-radius: 8px;
  padding: 15px;
  /* background-color: #ffffff; */
  /* box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); */
  height: 100%;
}

.flexibleSection {
  flex-grow: 1;
  overflow-y: auto;
  max-height: calc(100% - 100px); /* Adjust this value as needed */
}

.responseSection {
  padding: 15px;
  font-size: 0.8rem;
  background-color: #f9f9f9;
  border-left: 2px solid #0073e6;
  border-radius: 6px;
  position: relative;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.responseSection.expanded {
  overflow-y: auto;
}

.responseSection::-webkit-scrollbar {
  width: 6px;
}

.responseSection::-webkit-scrollbar-thumb {
  background-color: rgba(15, 95, 220, 1);
  border-radius: 10px;
}
