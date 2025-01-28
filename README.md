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
            <td>{new Date(doc.DateTime).toLocaleString()}</td>
            <td>
                <div className={styles.actionButtons}>
                    <button
                        className={styles.viewButton}
                        title="Preview Document"
                        onClick={() => handlePreviewClick(doc.presignedUrl)}
                    >
                        <FontAwesomeIcon icon={faEye} />
                    </button>
                </div>
            </td>
        </motion.tr>
    );
};








const handlePreviewClick = (url) => {
    setSelectedPreviewUrl(url);
    setIsPreviewModalOpen(true);
};

const closePreviewModal = () => {
    setSelectedPreviewUrl(null);
    setIsPreviewModalOpen(false);
};

return (
    <Modal
        isOpen={isPreviewModalOpen}
        onRequestClose={closePreviewModal}
        contentLabel="PDF Preview"
        className={styles.modal}
        overlayClassName={styles.overlay}
    >
        <h2>PDF Preview</h2>
        {selectedPreviewUrl ? (
            <iframe
                src={selectedPreviewUrl}
                title="PDF Preview"
                className={styles.pdfIframe}
                frameBorder="0"
            />
        ) : (
            <p>Loading preview...</p>
        )}
        <button onClick={closePreviewModal} className={styles.closeModalButton}>
            Close
        </button>
    </Modal>
);





.pdfIframe {
    width: 100%;
    height: 80vh;
    border: none;
}

.modal {
    background: #fff;
    border-radius: 8px;
    padding: 20px;
    max-width: 800px;
    margin: 0 auto;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.overlay {
    background: rgba(0, 0, 0, 0.5);
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
}

.closeModalButton {
    margin-top: 20px;
    padding: 10px 20px;
    background: #007bff;
    color: #fff;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}
