return (
  <div className={styles.mainContent}>
    {/* Left: Document Preview */}
    <div className={styles.previewSection}>
      <h3>Document Preview</h3>
      {data?.document_name?.length > 0 ? (
        <ul className={styles.documentList}>
          {data.document_name.map((doc, index) => (
            <li key={index}>
              <button
                className={styles.previewButton}
                onClick={() => setModalContent(doc.S)}
              >
                {doc.S}
              </button>
            </li>
          ))}
        </ul>
      ) : (
        <p>No document available</p>
      )}
    </div>

    {/* Right: Extracted Content */}
    <div className={styles.extractContentSection}>
      <h3>Extracted Content</h3>
      {loading ? (
        <p>Loading...</p>
      ) : (
        renderReadableContent(data?.total_extracted_data)
      )}
    </div>

    {/* Centered Verify Button */}
    <div className={styles.verifyButtonContainer}>
      <button
        className={styles.verifyButton}
        onClick={() => console.log("Verify action triggered")}
      >
        Verify
      </button>
    </div>

    {/* Modal for Document Preview */}
    <Modal
      isOpen={isModalOpen}
      onRequestClose={() => setModalContent(null)}
      className={styles.modal}
      overlayClassName={styles.modalOverlay}
      ariaHideApp={false}
    >
      <div className={styles.modalContent}>
        <button
          className={styles.closeButton}
          onClick={() => setModalContent(null)}
        >
          Close
        </button>
        {modalContent ? (
          <iframe
            src={modalContent}
            title="Preview"
            className={styles.modalIframe}
          ></iframe>
        ) : (
          <p>No preview available</p>
        )}
      </div>
    </Modal>
  </div>
);



.mainContent {
  display: grid;
  grid-template-columns: 1fr 1fr; /* Two equal sections */
  grid-template-rows: auto 50px; /* Rows for content and button */
  gap: 20px;
  padding: 20px;
  align-items: start; /* Align sections to the top */
  position: relative;
}

.previewSection,
.extractContentSection {
  background: #fff;
  padding: 15px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

.verifyButtonContainer {
  grid-column: span 2; /* Span across both columns */
  display: flex;
  justify-content: center; /* Center the button */
  align-items: center;
  margin-top: 20px;
}

.verifyButton {
  padding: 12px 24px;
  font-size: 16px;
  background-color: #28a745; /* Green */
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.verifyButton:hover {
  background-color: #218838; /* Darker green */
}
