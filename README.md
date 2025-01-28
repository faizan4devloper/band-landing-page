import React, { useState, useEffect } from 'react';
import axios from 'axios';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faEye } from '@fortawesome/free-solid-svg-icons';
import { motion, AnimatePresence } from 'framer-motion';
import Modal from 'react-modal';
import styles from './DashboardTable.module.css';

Modal.setAppElement('#root');

const DashboardTable = ({ userEmail }) => {
  const [documents, setDocuments] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [selectedDocUrl, setSelectedDocUrl] = useState('');
  const [isModalOpen, setIsModalOpen] = useState(false);

  const API_ENDPOINT = 'dummy-fetch-documents'; // Replace with your actual API endpoint for fetching documents
  const PREVIEW_API_ENDPOINT = 'dummy-preview-api'; // Replace with your actual API endpoint for generating preview URLs

  // Fetch documents on component mount
  const fetchDocuments = async () => {
    setLoading(true);
    setError(null);
    try {
      const response = await axios.post(API_ENDPOINT, {
        userEmail, // Include the user's email to fetch relevant documents
      });

      setDocuments(response.data.documents); // Replace with the actual response structure
    } catch (err) {
      setError('Failed to fetch documents.');
      setDocuments([]);
    } finally {
      setLoading(false);
    }
  };

  // Handle document preview
  const handlePreview = async (category, filename) => {
    try {
      const payload = {
        category,
        filename,
      };

      const response = await axios.post(PREVIEW_API_ENDPOINT, payload, {
        headers: { 'Content-Type': 'application/json' },
      });

      const { presignedUrl } = response.data;
      setSelectedDocUrl(presignedUrl);
      setIsModalOpen(true);
    } catch (err) {
      console.error('Failed to fetch preview URL:', err);
      alert('Unable to preview the document.');
    }
  };

  const renderDocumentRow = (doc) => (
    <motion.tr
      key={doc.id}
      initial={{ opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
      transition={{ duration: 0.3 }}
      className={styles.documentRow}
    >
      <td>{doc.filename}</td>
      <td>{doc.category}</td>
      <td>
        <button
          className={styles.viewButton}
          onClick={() => handlePreview(doc.category, doc.filename)}
        >
          <FontAwesomeIcon icon={faEye} /> Preview
        </button>
      </td>
    </motion.tr>
  );

  useEffect(() => {
    fetchDocuments();
  }, []);

  return (
    <div className={styles.documentTableContainer}>
      <table className={styles.documentTable}>
        <thead>
          <tr>
            <th>Filename</th>
            <th>Category</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <AnimatePresence>
            {loading ? (
              <tr>
                <td colSpan="3">Loading...</td>
              </tr>
            ) : documents.length > 0 ? (
              documents.map((doc) => renderDocumentRow(doc))
            ) : (
              <tr>
                <td colSpan="3">No documents found.</td>
              </tr>
            )}
          </AnimatePresence>
        </tbody>
      </table>

      {/* Modal for preview */}
      <Modal
        isOpen={isModalOpen}
        onRequestClose={() => setIsModalOpen(false)}
        className={styles.modal}
        overlayClassName={styles.overlay}
      >
        <iframe src={selectedDocUrl} title="PDF Preview" className={styles.pdfViewer}></iframe>
        <button className={styles.closeButton} onClick={() => setIsModalOpen(false)}>
          Close
        </button>
      </Modal>
    </div>
  );
};

export default DashboardTable;






.documentTableContainer {
  margin: 20px;
}

.documentTable {
  width: 100%;
  border-collapse: collapse;
}

.documentTable th,
.documentTable td {
  padding: 10px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

.documentRow:hover {
  background-color: #f1f1f1;
}

.viewButton {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 8px 12px;
  cursor: pointer;
  border-radius: 5px;
}

.viewButton:hover {
  background-color: #0056b3;
}

.modal {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 80%;
  height: 80%;
  background: white;
  padding: 20px;
  overflow: auto;
  border-radius: 10px;
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
}

.pdfViewer {
  width: 100%;
  height: calc(100% - 50px);
}

.closeButton {
  margin-top: 10px;
  padding: 8px 12px;
  background: red;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
