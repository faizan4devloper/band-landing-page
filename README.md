{isUploaded ? (
  <MainContent 
    message={message} 
    selectedPolicy={selectedPolicy}
    rows={rows} 
    clid={rows[0].recNum}
    setRows={setRows} 
    staticPreviewUrl={previewUrl} 
    uploadedFileName={uploadedFileName}
  />
) : (
  <div className={styles.emptyStateContainer}>
    <div className={styles.emptyStateBox}>
      <img 
        src="/assets/upload-placeholder.svg" 
        alt="Upload Document" 
        className={styles.uploadImage} 
      />
      <h3 className={styles.emptyStateTitle}>Upload a Document</h3>
      <p className={styles.emptyStateMessage}>
        No document found. Please upload a document to process and view the extracted data.
      </p>
      <button className={styles.uploadButton}>
        <FontAwesomeIcon icon={faUpload} className={styles.uploadButtonIcon} />
        Upload Now
      </button>
    </div>
  </div>
)}


.emptyStateContainer {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  text-align: center;
}

.emptyStateBox {
  background: white;
  padding: 24px;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  max-width: 420px;
  transition: all 0.3s ease-in-out;
}

.emptyStateBox:hover {
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.15);
}

.uploadImage {
  width: 100px;
  margin-bottom: 15px;
}

.emptyStateTitle {
  font-size: 20px;
  font-weight: 600;
  color: #333;
  margin-bottom: 8px;
}

.emptyStateMessage {
  font-size: 14px;
  color: #666;
  margin-bottom: 16px;
}

.uploadButton {
  background: #007bff;
  color: white;
  font-size: 14px;
  font-weight: 600;
  padding: 10px 18px;
  border: none;
  border-radius: 8px;
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
  transition: background 0.3s ease-in-out;
}

.uploadButton:hover {
  background: #0056b3;
}

.uploadButtonIcon {
  font-size: 16px;
}


<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 48 48" width="100" height="100">
  <g fill="none" stroke="#007bff" stroke-width="2">
    <path d="M24 4v24M24 4L17 11M24 4l7 7" />
    <path d="M6 28h36c1.1 0 2 .9 2 2v2c0 1.1-.9 2-2 2H6c-1.1 0-2-.9-2-2v-2c0-1.1.9-2 2-2z" />
  </g>
</svg>
