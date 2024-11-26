 return (
      <div className={styles.readableContent}>
        <h4>Claim Form Details</h4>
        <p><strong>Type:</strong> {parsedData.CLAIM_FORM_DETAILS?.CLAIM_FORM_TYPE}</p>

        <h4>Clinical Abstract Application</h4>
        <p><strong>Name of Patient:</strong> {parsedData.CLINICAL_ABSTRACT_APPLICATION?.NAME_OF_PATIENT}</p>
        <p><strong>NRIC/FIN/BC:</strong> {parsedData.CLINICAL_ABSTRACT_APPLICATION?.NRIC_FIN_BC}</p>
        <p><strong>Address:</strong> {parsedData.CLINICAL_ABSTRACT_APPLICATION?.ADDRESS}</p>
        <p><strong>Date:</strong> {parsedData.CLINICAL_ABSTRACT_APPLICATION?.DATE}</p>

        <h4>Claimant Statement</h4>
        <h5>Policy Details</h5>
        <p><strong>Policy Numbers:</strong> {parsedData.CLAIMANT_STATEMENT?.POLICY_DETAILS?.POLICY_NUMBERS}</p>
        <p><strong>Type of Claim:</strong> {parsedData.CLAIMANT_STATEMENT?.POLICY_DETAILS?.TYPE_OF_CLAIM}</p>

        <h5>Details of Life Assured</h5>
        <p><strong>Name:</strong> {parsedData.CLAIMANT_STATEMENT?.DETAILS_OF_LIFE_ASSURED?.NAME_OF_LIFE_ASSURED}</p>
        <p><strong>Gender:</strong> {parsedData.CLAIMANT_STATEMENT?.DETAILS_OF_LIFE_ASSURED?.GENDER}</p>
        <p><strong>Marital Status:</strong> {parsedData.CLAIMANT_STATEMENT?.DETAILS_OF_LIFE_ASSURED?.MARITAL_STATUS}</p>
      </div>
    );
  };

  return (
    <div className={styles.mainContent}>
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

      <div className={styles.extractContentSection}>
        <h3>Extracted Content</h3>
        {loading ? (
          <p>Loading...</p>
        ) : (
          renderReadableContent(data?.total_extracted_data)
        )}

        <button
          className={styles.reloadButton}
          onClick={() => handleReload("CL928697")}
          disabled={loading}
        >
          Reload
        </button>
        
      </div>

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
};



.mainContent {
  display: flex;
  gap: 20px;
  padding: 20px;
}

.previewSection,
.extractContentSection {
  flex: 1;
  padding: 10px;
  background: #fff;
  border: 1px solid #ddd;
  border-radius: 5px;
}

.documentList {
  list-style: none;
  padding: 0;
}

.documentList li {
  margin-bottom: 10px;
}

.previewButton {
  background-color: #007bff;
  color: white;
  padding: 8px 12px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.previewButton:hover {
  background-color: #0056b3;
}

.extractedData {
  background: #f8f8f8;
  padding: 10px;
  border-radius: 5px;
  overflow-x: auto;
}

.reloadButton {
  margin-top: 10px;
  background:linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
  color: white;
  padding: 8px 12px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.reloadButton:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

.readableContent {
  background: #f8f8f8;
  padding: 15px;
  border-radius: 8px;
  line-height: 1.6;
}

.readableContent h4 {
  color: #333;
  margin-bottom: 10px;
  text-decoration: underline;
}

.readableContent p {
  margin: 5px 0;
  color: #555;
}

.readableContent strong {
  color: #000;
}

h5 {
  margin-top: 15px;
  font-size: 1.1rem;
  color: #444;
}
