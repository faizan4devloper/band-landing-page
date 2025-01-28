<Modal
  isOpen={isDocumentPreviewOpen}
  onRequestClose={() => setIsDocumentPreviewOpen(false)}
  className={styles.modalContainer}
  overlayClassName={styles.overlay}
>
  {selectedDocument && (
    <div className={styles.modalContent}>
      {/* PDF Preview Section */}
      <div className={styles.previewSection}>
        <iframe 
          src={selectedDocument.url} 
          className={styles.previewIframe}
          title={selectedDocument.filename}
        />
      </div>

      {/* Metadata Section */}
      <div className={styles.metadataSection}>
        <h2 className={styles.documentTitle}>{selectedDocument.filename}</h2>
        <div className={styles.metadataContent}>
          <h3>Metadata</h3>
          <pre className={styles.metadataJson}>
            {selectedDocument.metadata
              ? JSON.stringify(selectedDocument.metadata, null, 2)
              : "No metadata available"}
          </pre>
        </div>
        <button 
          onClick={() => setIsDocumentPreviewOpen(false)} 
          className={styles.closeModalButton}
        >
          Close
        </button>
      </div>
    </div>
  )}
</Modal>






/* Modal Container */
.modalContainer {
  display: flex;
  width: 90%;
  max-width: 1200px;
  height: 80vh;
  background-color: #ffffff;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
}

/* Overlay */
.overlay {
  background-color: rgba(0, 0, 0, 0.6);
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}

/* Preview Section */
.previewSection {
  flex: 1.5;
  background-color: #f8f8f8;
  border-right: 2px solid #e0e0e0;
}

.previewIframe {
  width: 100%;
  height: 100%;
  border: none;
}

/* Metadata Section */
.metadataSection {
  flex: 1;
  padding: 20px;
  overflow-y: auto;
}

.documentTitle {
  font-size: 20px;
  font-weight: bold;
  color: #333;
  margin-bottom: 20px;
  text-align: center;
}

.metadataContent {
  background-color: #f9f9f9;
  padding: 15px;
  border-radius: 8px;
  border: 1px solid #e0e0e0;
  overflow-y: auto;
}

.metadataContent h3 {
  font-size: 18px;
  font-weight: bold;
  color: #6a11cb;
  margin-bottom: 10px;
}

.metadataJson {
  background-color: #f0f0f0;
  color: #333;
  font-size: 14px;
  font-family: monospace;
  padding: 10px;
  border-radius: 6px;
  white-space: pre-wrap;
  overflow-wrap: break-word;
  border: 1px solid #ddd;
}

/* Close Button */
.closeModalButton {
  background-color: #e53935;
  color: #ffffff;
  border: none;
  padding: 10px 20px;
  border-radius: 6px;
  cursor: pointer;
  font-size: 14px;
  display: block;
  margin: 20px auto 0;
  text-align: center;
  transition: background-color 0.3s ease;
}

.closeModalButton:hover {
  background-color: #d32f2f;
}
