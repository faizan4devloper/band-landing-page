// DashboardTable.js
import React, { useState, useEffect } from 'react';
import axios from 'axios';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import {
  faSync, faFileAlt, faEye, faDownload, faRotate
} from '@fortawesome/free-solid-svg-icons';
import { motion, AnimatePresence } from 'framer-motion';
import DocumentModal from './DocumentModal'; // Import the new DocumentModal component
import styles from './DashboardTable.module.css';

const DashboardTable = ({ userEmail }) => {
  const [documents, setDocuments] = useState([]);
  const [loading, setLoading] = useState(false);
  const [currentPage, setCurrentPage] = useState(0);
  const [nextStartKey, setNextStartKey] = useState(null);
  const [selectedDocument, setSelectedDocument] = useState(null);
  const [isDocumentPreviewOpen, setIsDocumentPreviewOpen] = useState(false);

  const API_ENDPOINT = 'https://'; // Replace with actual endpoint

  const fetchDocuments = async (isRefresh = false) => {
    setLoading(true);
    try {
      const payload = {
        queryStringParameters: isRefresh ? { refresh: true } : { start_page: currentPage, page_size: 2 }
      };
      const response = await axios.post(API_ENDPOINT, payload, { headers: { 'Content-Type': 'application/json' } });
      // handle parsing and setting the documents...
      setDocuments(parsedData); // Assuming `parsedData` is the data structure you get from the API
    } catch (err) {
      console.error(err);
    } finally {
      setLoading(false);
    }
  };

  useEffect(() => {
    fetchDocuments();
  }, [currentPage]);

  const handleViewDocument = (doc) => {
    // Set the document and open the preview modal
    setSelectedDocument(doc);
    setIsDocumentPreviewOpen(true);
  };

  const renderTableContent = () => {
    if (loading) {
      return (
        <tr>
          <td colSpan="5">Loading documents...</td>
        </tr>
      );
    }
    if (documents.length === 0) {
      return (
        <tr>
          <td colSpan="5">No documents found</td>
        </tr>
      );
    }
    return documents.map(doc => (
      <motion.tr key={doc.TransactionID}>
        <td>{doc.Filename}</td>
        <td>{doc.Category}</td>
        <td>{new Date(doc.DateTime).toLocaleString()}</td>
        <td>
          <button onClick={() => handleViewDocument(doc)}>
            <FontAwesomeIcon icon={faEye} />
          </button>
          <button>
            <FontAwesomeIcon icon={faDownload} />
          </button>
        </td>
      </motion.tr>
    ));
  };

  const handlePageChange = (direction) => {
    if (direction === 'next' && documents.length > 0) {
      setCurrentPage(prev => prev + 1);
    } else if (direction === 'prev' && currentPage > 0) {
      setCurrentPage(prev => Math.max(0, prev - 1));
    }
  };

  return (
    <div className={styles.documentTableContainer}>
      <div className={styles.tableHeader}>
        <h3>Dashboard</h3>
        <motion.button onClick={() => fetchDocuments(true)} whileHover={{ scale: 1.05 }}>
          <FontAwesomeIcon icon={faRotate} /> Refresh
        </motion.button>
      </div>

      <table>
        <thead>
          <tr>
            <th>Filename</th>
            <th>Category</th>
            <th>Date</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <AnimatePresence>{renderTableContent()}</AnimatePresence>
        </tbody>
      </table>

      {/* Pagination */}
      <div>
        <button onClick={() => handlePageChange('prev')}>Previous</button>
        <span>Page {currentPage + 1}</span>
        <button onClick={() => handlePageChange('next')}>Next</button>
      </div>

      {/* Document Preview Modal */}
      {isDocumentPreviewOpen && (
        <DocumentModal
          document={selectedDocument}
          closeModal={() => setIsDocumentPreviewOpen(false)}
        />
      )}
    </div>
  );
};

export default DashboardTable;





// DocumentModal.js
import React from 'react';
import Modal from 'react-modal';
import styles from './DocumentModal.module.css';

const DocumentModal = ({ document, closeModal }) => {
  return (
    <Modal isOpen={true} onRequestClose={closeModal} className={styles.modalContent} overlayClassName={styles.overlay}>
      <div className={styles.previewSection}>
        <iframe src={document.url} title={document.filename} />
      </div>
      <div className={styles.metadataSection}>
        <h2>{document.filename}</h2>
        <h3>Document Metadata</h3>
        <pre>{JSON.stringify(document.metadata, null, 2)}</pre>
        <button onClick={closeModal}>Close</button>
      </div>
    </Modal>
  );
};

export default DocumentModal;
