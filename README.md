the Modal look poor i want add styling consitent and flexible proper balancing both pdf and meta are in same width and height pls redesing it and i want the meta data display in the json format:- 

<Modal
  isOpen={isDocumentPreviewOpen}
  onRequestClose={() => setIsDocumentPreviewOpen(false)}
  className={styles.documentPreviewModal}
  overlayClassName={styles.overlay}
>
  {selectedDocument && (
    <div className={styles.documentPreviewModal}>
      <div className={styles.previewLeftSide}>
        <iframe 
          src={selectedDocument.url} 
          className={styles.previewIframe}
          title={selectedDocument.filename}
        />
      </div>
      <div className={styles.previewRightSide}>
        <h2>{selectedDocument.filename}</h2>
        <div className={styles.metadataSection}>
          <h3>Metadata Details</h3>
          {selectedDocument.metadata ? (
            Object.entries(selectedDocument.metadata).map(([key, value]) => (
              <div key={key} className={styles.metadataItem}>
                <span>{key}</span>
                <span>{value}</span>
              </div>
            ))
          ) : (
            <p>No metadata available</p>
          )}
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

.filenameButton{
  background:none;
  border:none;
  color:#007bff;
  text-decoration: underline;
  cursor: pointer;
  font-size: 14px;
}


.documentPreviewModal {
  display: flex;
  width: 90%;
  max-width: 1200px;
  height: 80vh;
  background-color: white;
  border-radius: 12px;
  overflow: hidden;
}

.previewLeftSide {
  flex: 2;
  background-color: #f0f0f0;
}

.previewRightSide {
  flex: 1;
  padding: 20px;
  background-color: white;
  overflow-y: auto;
}

.previewIframe {
  width: 100%;
  height: 100%;
  border: none;
}

.metadataSection {
  margin-bottom: 15px;
}

.metadataSection h3 {
  margin-bottom: 10px;
  color: #333;
  border-bottom: 2px solid #6a11cb;
  padding-bottom: 5px;
}

.metadataItem {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
  padding: 8px;
  background-color: #f9f9f9;
  border-radius: 4px;
}

.metadataItem span:first-child {
  font-weight: bold;
  color: #6a11cb;
}

.metadataItem span:last-child {
  color: #666;
}
