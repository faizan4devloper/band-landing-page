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



/* DashboardTable.module.css */

.documentTableContainer {
  background-color: #ffffff;
  border-radius: 12px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 20px;
  max-width: 1200px;
  margin: 0 auto;
}

.tableHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.tableHeader h3 {
  font-size: 24px;
  font-weight: 600;
  color: #333;
}

.headerActions {
  display: flex;
  align-items: center;
}

.refreshButton {
  background-color: #6a11cb;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  font-size: 14px;
  display: flex;
  align-items: center;
  gap: 8px;
}

.refreshButton:hover {
  background-color: #45a049;
}

.refreshButton:disabled {
  background-color: #cccccc;
  cursor: not-allowed;
}

.documentTable {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

.documentTable th,
.documentTable td {
  border: 1px solid #ddd;
  padding: 12px 16px;
  text-align: left;
  font-size: 14px;
  color: #333;
}

.documentTable th {
  background-color: #f4f4f4;
  font-weight: 600;
}

.documentTable tr:nth-child(even) {
  background-color: #f9f9f9;
}

.documentTable tr:hover {
  background-color: #e2e2e2;
}

.documentRow td {
  font-size: 14px;
}

.actionButtons {
  display: flex;
  gap: 12px;
}

.viewButton, .downloadButton {
  background-color: #6a11cb;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.viewButton:hover, .downloadButton:hover {
  background-color: #0056b3;
}

.seeMoreButton {
  background-color: #6a11cb;
  color: white;
  border: none;
  padding: 6px 12px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}

.seeMoreButton:hover {
  background-color: #6a11ca;
}

.emptyRow {
  text-align: center;
}

.emptyContent {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 20px;
}

.emptyIcon {
  font-size: 48px;
  color: #ccc;
}

.loadingRow {
  text-align: center;
}

.loadingContent {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 20px;
}

.loadingIcon {
  font-size: 36px;
  margin-right: 8px;
}

.pagination {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin-top: 20px;
}

.pagination button {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.pagination button:disabled {
  background-color: #cccccc;
  cursor: not-allowed;
}

.pagination button:hover {
  background-color: #0056b3;
}

.modal {
  background-color: white;
  padding: 20px;
  border-radius: 8px;
  max-width: 600px;
  margin: 0 auto;
}

.modal h2 {
  font-size: 18px;
  font-weight: 600;
  color: #333;
}

.closeModalButton {
  background-color: #f44336;
  color: white;
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin-top: 20px;
  font-size: 14px;
}

.closeModalButton:hover {
  background-color: #e53935;
}

.overlay {
  background-color: rgba(0, 0, 0, 0.5);
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}
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
