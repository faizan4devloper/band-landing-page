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
