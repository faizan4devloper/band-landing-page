const renderDocumentRow = (doc) => {
  const displayFields = {
    AccountStatement: [doc.Metadata['Account No.'] || 'N/A', doc.Metadata['Date'] || 'N/A'],
    CreditNote: [doc.Metadata['Credit Note'] || 'N/A', doc.Metadata['Date/Tax Point'] || 'N/A'],
  };

  return (
    <motion.tr
      key={doc.TransactionID}
      initial={{ opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
      transition={{ duration: 0.3 }}
      className={styles.documentRow}
    >
      {/* Clickable Filename */}
      <td>
        <a
          href={doc.Filename} // Assumes `Filename` is the URL to the PDF
          target="_blank"
          rel="noopener noreferrer"
          className={styles.filenameLink}
        >
          {doc.Filename.split('/').pop()}
        </a>
      </td>
      <td>{doc.Category}</td>
      <td>
        <button className={styles.seeMoreButton} onClick={() => handleMetadataClick(doc.Metadata)}>
          See More
        </button>
      </td>
      <td>{new Date(doc.DateTime).toLocaleString()}</td>
      <td>
        <div className={styles.actionButtons}>
          <button className={styles.viewButton} title="View Document">
            <FontAwesomeIcon icon={faEye} />
          </button>
          <button className={styles.downloadButton} title="Download Document">
            <FontAwesomeIcon icon={faDownload} />
          </button>
        </div>
      </td>
    </motion.tr>
  );
};
