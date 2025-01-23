const renderDocumentRow = (doc) => {
  const displayFields = {
    'AccountStatement': [doc.Metadata['Account No.'] || 'N/A', doc.Metadata['Date'] || 'N/A'],
    'CreditNote': [doc.Metadata['Credit Note'] || 'N/A', doc.Metadata['Date/Tax Point'] || 'N/A']
  };

  return (
    <motion.tr
      key={doc.TransactionID}
      initial={{ opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
      transition={{ duration: 0.3 }}
      className={styles.documentRow}
    >
      {/* Link to open the PDF */}
      <td>
        <a href={`https://${doc.Filename}`} target="_blank" rel="noopener noreferrer">
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







Parsed Body: Object
data
: 
Array(3)
0
: 
page_number
: 
1
transactions
: 
Array(2)
0
: 
Category
: 
{S: 'AccountStatement'}
DateTime
: 
{S: '2025-01-08T10:31:08.749612'}
Filename
: 
{S: 'processed/Statement_data_1_1.pdf'}
Metadata
: 
{M: {…}}
Status
: 
{S: 'Success'}
TransactionID
: 
{S: '7dfb21f1-d4c8-4c39-9fa3-d1ab4f9623cd'}
