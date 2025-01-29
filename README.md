const renderDocumentRow = (doc) => {
  return (
    <motion.tr
      key={doc.TransactionID}
      initial={{ opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
      transition={{ duration: 0.3 }}
      className={styles.documentRow}
    >
      <td>{doc.Filename.split('/').pop()}</td>
      <td>{doc.Category}</td>
      <td>
        <button className={styles.seeMoreButton} onClick={() => handleMetadataClick(doc.Metadata)}>
          See More
        </button>
      </td>
      <td>{new Date(doc.DateTime).toLocaleString()}</td>
      <td>
        <div className={styles.actionButtons}>
          <button 
  className={styles.viewButton} 
  title="View Document"
  onClick={() => handleViewDocument(doc.Category, doc.Filename.split('/').pop())}
>
  <FontAwesomeIcon icon={faEye} />
</button>

          <button 
            className={styles.downloadButton} 
            title="Download Document"
            onClick={() => handleDownloadDocument(doc.Category, doc.Filename.split('/').pop())}
          >
            <FontAwesomeIcon icon={faDownload} />
          </button>
        </div>
      </td>
    </motion.tr>
  );
};

  const handleMetadataClick = (metadata) => {
    setSelectedMetadata(metadata);
    setIsModalOpen(true);
  };

  const closeModal = () => {
    setIsModalOpen(false);
  };

  const renderTableContent = () => {
    if (loading) {
      return (
        <tr>
          <td colSpan="6" className={styles.loadingRow}>
            <div className={styles.loadingContent}>
              <FontAwesomeIcon icon={faSync} spin className={styles.loadingIcon} />
              <p>Loading documents...</p>
            </div>
          </td>
        </tr>
      );
    }

    if (documents.length === 0) {
      return (
        <tr>
          <td colSpan="6" className={styles.emptyRow}>
            <div className={styles.emptyContent}>
              <FontAwesomeIcon icon={faFileAlt} className={styles.emptyIcon} />
              <p>No documents found</p>
            </div>
          </td>
        </tr>
      );
    }

    return documents.map(renderDocumentRow);
  };

  const handlePageChange = (direction) => {
    if (direction === 'next' && documents.length > 0) {
      setCurrentPage((prev) => prev + 1);
    } else if (direction === 'prev' && currentPage > 0) {
      setCurrentPage((prev) => Math.max(0, prev - 1));
    }
  };

  const handleRefresh = () => {
    fetchDocuments(true);
  };

  return (
    <div className={styles.documentTableContainer}>
      <div className={styles.tableHeader}>
        <h3>Dashboard</h3>
        <div className={styles.headerActions}>
          <motion.button
            onClick={handleRefresh}
            className={styles.refreshButton}
            whileHover={{ scale: 1.05 }}
            whileTap={{ scale: 0.95 }}
          >
            <FontAwesomeIcon icon={faRotate} />
            Refresh
          </motion.button>
        </div>
      </div>

      <div className={styles.tableWrapper}>
        <table className={styles.documentTable}>
          <thead>
            <tr>
              <th>Filename</th>
              <th>Category</th>
              <th>Metadata</th>
              <th>Date</th>
              <th>Processed At</th>
            </tr>
          </thead>
          <tbody>
            <AnimatePresence>{renderTableContent()}</AnimatePresence>
          </tbody>
        </table>
      </div>

      {/* Pagination */}
      <div className={styles.pagination}>
        <button onClick={() => handlePageChange('prev')} disabled={currentPage === 0}>
          Previous
        </button>
        <span>Page {currentPage + 1}</span>
        <button onClick={() => handlePageChange('next')} disabled={!nextStartKey}>
          Next
        </button>
      </div>

      {/* Modal for Metadata */}
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
        <div className={styles.previewWrapper}>
          <iframe 
            src={selectedDocument.url} 
            className={styles.previewIframe}
            title={selectedDocument.filename}
          />
        </div>
      </div>

      {/* Metadata Section */}
      <div className={styles.metadataSection}>
        <div className={styles.metadataHeader}>
          <h2 className={styles.documentTitle}>
            {selectedDocument.filename}
          </h2>
        </div>
        
        <div className={styles.metadataContent}>
          <h3>Document Metadata</h3>
          <pre className={styles.metadataJson}>
            {selectedDocument.metadata
              ? JSON.stringify(selectedDocument.metadata, null, 2)
              : "No metadata available"}
          </pre>
        </div>
        
        <div className={styles.modalActions}>
          <button 
            onClick={() => setIsDocumentPreviewOpen(false)} 
            className={styles.closeModalButton}
          >
            <FontAwesomeIcon icon={faXmark}/>
          </button>
        </div>
      </div>
    </div>
  )}
</Modal>




    </div>
  );
};



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
  background-color: #0056b3;
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
  background-color: #0056b3;
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

.modalContainer {
  outline: none;
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

#toolbar {
    align-items: center;
    background-color: var(--viewer-pdf-toolbar-background-color);
    color: rgb(255, 255, 255);
    display: flex
;
    /* height: var(--viewer-pdf-toolbar-height); */
    padding: 0px 16px;
    display: none;
}

.modalContent {
  display: flex;
  width: 100%;
  width: 1000px;
  height: 80vh;
  height: 560px;
  background-color: #ffffff;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
}

.previewSection {
  flex: 3;
  background-color: #f4f4f4;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
}

.previewWrapper {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.previewIframe {
  width: 95%;
  height: 95%;
  border: none;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.metadataSection {
  flex: 2;
  display: flex;
  flex-direction: column;
  padding: 30px;
  background-color: #ffffff;
  overflow: hidden;
}

.metadataHeader {
  text-align: center;
  margin-bottom: 20px;
  border-bottom: 2px solid #f0f0f0;
  padding-bottom: 15px;
}

.documentTitle {
  font-size: 22px;
  font-weight: 600;
  color: #333;
  margin: 0;
}

.metadataContent {
  flex-grow: 1;
  overflow-y: auto;
  background-color: #f9f9f9;
  border-radius: 10px;
  padding: 20px;
  margin-bottom: 20px;
}

.metadataContent h3 {
  font-size: 18px;
  color: #6a11cb;
  margin-bottom: 15px;
}

.metadataJson {
  background-color: #f0f0f0;
  color: #333;
  font-size: 14px;
  font-family: 'Courier New', monospace;
  padding: 15px;
  border-radius: 8px;
  white-space: pre-wrap;
  overflow-wrap: break-word;
  max-height: 300px;
  overflow-y: auto;
}

.modalActions {
  display: flex;
  justify-content: center;
}

.closeModalButton {
        background-color: #e53935;
    position: absolute;
    top: 12px;
    right: 131px;
    cursor: pointer;
    color: #ffffff;
    border: none;
    padding: 9px 19px;
    border-radius: 0px 16px 0px 0px;
    font-size: 16px;
    transition: all 0.3s ease;
}

.closeModalButton:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
}

/* Responsive Adjustments */
@media (max-width: 768px) {
  .modalContent {
    flex-direction: column;
    width: 95%;
    height: 90vh;
  }

  .previewSection,
  .metadataSection {
    flex: none;
    width: 100%;
  }
}
