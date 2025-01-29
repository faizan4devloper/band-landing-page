import React from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faSync, faRotate } from '@fortawesome/free-solid-svg-icons';
import { motion } from 'framer-motion';
import DocumentRow from './DocumentRow';
import LoadingState from './LoadingState';
import ErrorState from './ErrorState';
import styles from './DashboardTable.module.css';

const DocumentTable = ({ documents, loading, error, handleViewDocument, handleDownloadDocument, handlePageChange, currentPage, nextStartKey, handleRefresh }) => {
  const renderTableContent = () => {
    if (loading) return <LoadingState />;
    if (error) return <ErrorState message={error} />;
    if (documents.length === 0) return <ErrorState message="No documents found" />;

    return documents.map(doc => (
      <DocumentRow
        key={doc.TransactionID}
        doc={doc}
        handleViewDocument={handleViewDocument}
        handleDownloadDocument={handleDownloadDocument}
      />
    ));
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
          <tbody>{renderTableContent()}</tbody>
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
    </div>
  );
};

export default DocumentTable;




import React from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faEye, faDownload } from '@fortawesome/free-solid-svg-icons';
import styles from './DashboardTable.module.css';

const DocumentRow = ({ doc, handleViewDocument, handleDownloadDocument }) => (
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
      <button className={styles.seeMoreButton} onClick={() => handleViewDocument(doc.Category, doc.Filename.split('/').pop())}>
        See More
      </button>
    </td>
    <td>{new Date(doc.DateTime).toLocaleString()}</td>
    <td>
      <div className={styles.actionButtons}>
        <button className={styles.viewButton} title="View Document" onClick={() => handleViewDocument(doc.Category, doc.Filename.split('/').pop())}>
          <FontAwesomeIcon icon={faEye} />
        </button>
        <button className={styles.downloadButton} title="Download Document" onClick={() => handleDownloadDocument(doc.Category, doc.Filename.split('/').pop())}>
          <FontAwesomeIcon icon={faDownload} />
        </button>
      </div>
    </td>
  </motion.tr>
);

export default DocumentRow;






import React from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faSync } from '@fortawesome/free-solid-svg-icons';
import styles from './DashboardTable.module.css';

const LoadingState = () => (
  <tr>
    <td colSpan="6" className={styles.loadingRow}>
      <div className={styles.loadingContent}>
        <FontAwesomeIcon icon={faSync} spin className={styles.loadingIcon} />
        <p>Loading documents...</p>
      </div>
    </td>
  </tr>
);

export default LoadingState;



import React from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faFileAlt } from '@fortawesome/free-solid-svg-icons';
import styles from './DashboardTable.module.css';

const ErrorState = ({ message }) => (
  <tr>
    <td colSpan="6" className={styles.emptyRow}>
      <div className={styles.emptyContent}>
        <FontAwesomeIcon icon={faFileAlt} className={styles.emptyIcon} />
        <p>{message}</p>
      </div>
    </td>
  </tr>
);

export default ErrorState;




import React, { useState, useEffect } from 'react';
import axios from 'axios';
import Modal from 'react-modal';
import DocumentTable from './DocumentTable';
import DocumentPreviewModal from './DocumentPreviewModal'; // The modal component you created
import styles from './DashboardTable.module.css';

Modal.setAppElement('#root');

const DashboardTable = ({ userEmail }) => {
  const [documents, setDocuments] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [currentPage, setCurrentPage] = useState(0);
  const [nextStartKey, setNextStartKey] = useState(null);
  const [selectedDocument, setSelectedDocument] = useState(null);
  const [isDocumentPreviewOpen, setIsDocumentPreviewOpen] = useState(false);

  const API_ENDPOINT = 'https://'; // Replace with actual API

  const fetchDocuments = async (isRefresh = false) => {
    // Fetch logic remains the same
  };

  useEffect(() => {
    fetchDocuments();
  }, [currentPage]);

  const handleViewDocument = async (category, filename) => {
    // Document view logic remains the same
  };

  const handleDownloadDocument = async (category, filename) => {
    // Document download logic remains the same
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
      <DocumentTable
        documents={documents}
        loading={loading}
        error={error}
        handleViewDocument={handleViewDocument}
        handleDownloadDocument={handleDownloadDocument}
        handlePageChange={handlePageChange}
        currentPage={currentPage}
        nextStartKey={nextStartKey}
        handleRefresh={handleRefresh}
      />

      <DocumentPreviewModal
        isOpen={isDocumentPreviewOpen}
        document={selectedDocument}
        onClose={() => setIsDocumentPreviewOpen(false)}
      />
    </div>
  );
};

export default DashboardTable;
