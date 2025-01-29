
this component and code in one file is to much make another file or component:- import React, { useState, useEffect } from 'react'; import axios from 'axios'; import { FontAwesomeIcon } from '@fortawesome/react-fontawesome'; import { faSync, faFileAlt, faEye, faDownload, faFilter, faRotate } from '@fortawesome/free-solid-svg-icons';

import { motion, AnimatePresence } from 'framer-motion'; import Modal from 'react-modal'; import styles from './DashboardTable.module.css';

// Set app element for accessibility Modal.setAppElement('#root');

const DashboardTable = ({ userEmail }) => { const [documents, setDocuments] = useState([]); const [loading, setLoading] = useState(false); const [error, setError] = useState(null); const [currentPage, setCurrentPage] = useState(0); const [pageSize] = useState(2); const [nextStartKey, setNextStartKey] = useState(null); const [selectedMetadata, setSelectedMetadata] = useState(null); const [isModalOpen, setIsModalOpen] = useState(false);

const [selectedDocument, setSelectedDocument] = useState(null); const [isDocumentPreviewOpen, setIsDocumentPreviewOpen] = useState(false);

const API_ENDPOINT = 'https:dummy'; // Replace with the actual API endpoint

const fetchDocuments = async (isRefresh = false) => { setLoading(true); setError(null); try { const payload = { queryStringParameters: isRefresh ? { refresh: true, page_size: pageSize } : { start_page: currentPage, page_size: pageSize, ...(nextStartKey && { next_start_key: nextStartKey }) } };

  const response = await axios.post(API_ENDPOINT, payload, {
    headers: { 'Content-Type': 'application/json' }
  });
  

  const parsedBody = parseResponse(response.data);
  
  console.log('Raw Response:', response.data);
  console.log('Parsed Body:', parsedBody);


  console.log(parsedBody)
  if (!parsedBody || !parsedBody.data) {
    throw new Error('No data in response');
  }

  const allTransactions = parsedBody.data.flatMap((page) =>
    page.transactions.map((transaction) => {
      try {
        return {
          Filename: transaction.Filename?.S || 'Unknown',
          Category: transaction.Category?.S || 'Unknown',
          DateTime: transaction.DateTime?.S || new Date().toISOString(),
          TransactionID: transaction.TransactionID?.S || 'Unknown',
          Metadata: Object.fromEntries(
            Object.entries(transaction.Metadata?.M || {}).map(([key, value]) => [key, value.S || 'N/A'])
          )
        };
      } catch {
        return null;
      }
    }).filter(Boolean)
  );

  setDocuments(allTransactions);
  setNextStartKey(parsedBody.next_start_key || null);
} catch (err) {
  setError(err.message || 'Failed to fetch documents');
  setDocuments([]);
} finally {
  setLoading(false);
}
};

useEffect(() => { fetchDocuments(); }, [currentPage]);

const parseResponse = (response) => { try { const responseData = typeof response === 'string' ? JSON.parse(response) : response; return typeof responseData.body === 'string' ? JSON.parse(responseData.body) : responseData.body; } catch { setError('Failed to parse response'); return null; } };

// console.log('Parsed Data:', parseResponse)

const handleViewDocument = async (category, filename) => { try { const presignedUrlPayload = { category: category, filename: filename };

// Make a POST request to get the presigned URL
const presignedUrlResponse = await axios.post(
  'https://dummy', 
  presignedUrlPayload
);

console.log('PresignUpload Raw Response:', presignedUrlResponse);

// Parse the body which is a string
const responseBody = JSON.parse(presignedUrlResponse.data.body);

// Extract the presigned URL 
const presignedUrl = responseBody.presigned_url;

console.log('Preview Presignurl:', presignedUrl);

// Open the PDF in a new tab
if (presignedUrl) {
  setSelectedDocument({
    url: presignedUrl,
    filename: filename,
    metadata: documents.find(doc => doc.Filename.split('/').pop() === filename)?.Metadata
  });
  setIsDocumentPreviewOpen(true);
} else {
  throw new Error('No presigned URL found');
}
} catch (error) { console.error('Error fetching presigned URL:', error); alert('Failed to view document'); } };

const handleDownloadDocument = async (category, filename) => { try { const presignedUrlPayload = { category: category, filename: filename };

// Make a POST request to get the presigned URL
const presignedUrlResponse = await axios.post(
  'https:dummy', 
  presignedUrlPayload
);

// Parse the body which is a string
const responseBody = JSON.parse(presignedUrlResponse.data.body);

// Extract the presigned URL
const presignedUrl = responseBody.presigned_url;

// Create a temporary anchor element to trigger download
if (presignedUrl) {
  const link = document.createElement('a');
  link.href = presignedUrl;
  link.download = filename; // Set the filename for download
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
} else {
  throw new Error('No presigned URL found');
}
} catch (error) { console.error('Error fetching presigned URL:', error); // Optionally show an error message to the user alert('Failed to download document'); } };

const renderDocumentRow = (doc) => { return ( <motion.tr key={doc.TransactionID} initial={{ opacity: 0, y: 20 }} animate={{ opacity: 1, y: 0 }} transition={{ duration: 0.3 }} className={styles.documentRow} > <td>{doc.Filename.split('/').pop()}</td> <td>{doc.Category}</td> <td> <button className={styles.seeMoreButton} onClick={() => handleMetadataClick(doc.Metadata)}> See More </button> </td> <td>{new Date(doc.DateTime).toLocaleString()}</td> <td> <div className={styles.actionButtons}> <button className={styles.viewButton} title="View Document" onClick={() => handleViewDocument(doc.Category, doc.Filename.split('/').pop())}

<FontAwesomeIcon icon={faEye} /> </button
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
); };

const handleMetadataClick = (metadata) => { setSelectedMetadata(metadata); setIsModalOpen(true); };

const closeModal = () => { setIsModalOpen(false); };

const renderTableContent = () => { if (loading) { return ( <tr> <td colSpan="6" className={styles.loadingRow}> <div className={styles.loadingContent}> <FontAwesomeIcon icon={faSync} spin className={styles.loadingIcon} /> <p>Loading documents...</p> </div> </td> </tr> ); }

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

const handlePageChange = (direction) => { if (direction === 'next' && documents.length > 0) { setCurrentPage((prev) => prev + 1); } else if (direction === 'prev' && currentPage > 0) { setCurrentPage((prev) => Math.max(0, prev - 1)); } };

const handleRefresh = () => { fetchDocuments(true); };

return ( <div className={styles.documentTableContainer}> <div className={styles.tableHeader}> <h3>Dashboard</h3> <div className={styles.headerActions}> <motion.button onClick={handleRefresh} className={styles.refreshButton} whileHover={{ scale: 1.05 }} whileTap={{ scale: 0.95 }} > <FontAwesomeIcon icon={faRotate} /> Refresh </motion.button> </div> </div>

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
isOpen={isDocumentPreviewOpen} onRequestClose={() => setIsDocumentPreviewOpen(false)} className={styles.modalContainer} overlayClassName={styles.overlay}

{selectedDocument && ( <div className={styles.modalContent}> {/* PDF Preview Section */} <div className={styles.previewSection}> <div className={styles.previewWrapper}> <iframe src={selectedDocument.url} className={styles.previewIframe} title={selectedDocument.filename} /> </div> </div>

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
        Close Preview
      </button>
    </div>
  </div>
</div>
)} </Modal>

</div>
); };

export default DashboardTable;
