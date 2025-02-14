<div className={styles.infoContainer}>
    <div className={styles.infoBox}>
      <FontAwesomeIcon icon={faCloudUploadAlt} className={styles.uploadIcon} />
      <h3 className={styles.infoTitle}>No Document Uploaded</h3>
      <p className={styles.infoMessage}>
        Please upload a document to view the extracted data and details.
      </p>
    </div>
  </div>


  .infoContainer {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  text-align: center;
}

.infoBox {
  background: #f9f9f9;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  max-width: 400px;
}

.uploadIcon {
  font-size: 40px;
  color: #007bff;
  margin-bottom: 10px;
}

.infoTitle {
  font-size: 18px;
  font-weight: 600;
  color: #333;
  margin-bottom: 5px;
}

.infoMessage {
  font-size: 14px;
  color: #666;
}
