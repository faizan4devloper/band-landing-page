import React, { useState, useEffect } from 'react';
import axios from 'axios';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faSync, faFileAlt, faDownload, faRotateRight } from '@fortawesome/free-solid-svg-icons';
import { motion, AnimatePresence } from 'framer-motion';
import Modal from 'react-modal';
import styles from './DashboardTable.module.css';

// Set app element for accessibility
Modal.setAppElement('#root');

const DashboardTable = ({ userEmail }) => {
  const [documents, setDocuments] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [currentPage, setCurrentPage] = useState(0);
  const [pageSize] = useState(2);
  const [nextStartKey, setNextStartKey] = useState(null);
  const [isModalOpen, setIsModalOpen] = useState(false);
  const [selectedFileURL, setSelectedFileURL] = useState(null);

  const API_ENDPOINT = 'dummy'; // Replace with the actual API endpoint

  const fetchDocuments = async () => {
    setLoading(true);
    setError(null);
    try {
      const payload = {
        queryStringParameters: {
          start_page: currentPage,
          page_size: pageSize,
          ...(nextStartKey && { next_start_key: nextStartKey })
        }
      };

      const response = await axios.post(API_ENDPOINT, payload, {
        headers: { 'Content-Type': 'application/json' }
      });

      const parsedBody = parseResponse(response.data);

      if (!parsedBody || !parsedBody.data) {
        throw new Error('No data in response');
      }

      const allTransactions = parsedBody.data.flatMap((page) =>
        page.transactions.map((transaction) => ({
          Filename: transaction.Filename?.S || 'Unknown',
          FileURL: transaction.FileURL?.S || '#', // Add file URL here
          Category: transaction.Category?.S || 'Unknown',
          DateTime: transaction.DateTime?.S || new Date().toISOString(),
          TransactionID: transaction.TransactionID?.S || 'Unknown',
        }))
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

  useEffect(() => {
    fetchDocuments();
  }, [currentPage]);

  const parseResponse = (response) => {
    try {
      const responseData = typeof response === 'string' ? JSON.parse(response) : response;
      return typeof responseData.body === 'string' ? JSON.parse(responseData.body) : responseData.body;
    } catch {
      setError('Failed to parse response');
      return null;
    }
  };

  const openModal = (fileURL) => {
    setSelectedFileURL(fileURL);
    setIsModalOpen(true);
  };

  const closeModal = () => {
    setSelectedFileURL(null);
    setIsModalOpen(false);
  };

  const renderDocumentRow = (doc) => (
    <motion.tr
      key={doc.TransactionID}
      initial={{ opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
      transition={{ duration: 0.3 }}
      className={styles.documentRow}
    >
      <td>
        <button className={styles.filenameButton} onClick={() => openModal(doc.FileURL)}>
          {doc.Filename.split('/').pop()}
        </button>
      </td>
      <td>{doc.Category}</td>
      <td>{new Date(doc.DateTime).toLocaleString()}</td>
      <td>
        <button className={styles.downloadButton} title="Download Document">
          <FontAwesomeIcon icon={faDownload} />
        </button>
      </td>
    </motion.tr>
  );

  const renderTableContent = () => {
    if (loading) {
      return (
        <tr>
          <td colSpan="4" className={styles.loadingRow}>
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
          <td colSpan="4" className={styles.emptyRow}>
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

  return (
    <div className={styles.documentTableContainer}>
      <h3>Dashboard</h3>
      <table className={styles.documentTable}>
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

      {/* Modal for File Preview */}
      <Modal
        isOpen={isModalOpen}
        onRequestClose={closeModal}
        contentLabel="File Preview"
        className={styles.modal}
        overlayClassName={styles.overlay}
      >
        {selectedFileURL ? (
          <iframe
            src={selectedFileURL}
            title="Document Preview"
            width="100%"
            height="500px"
            frameBorder="0"
          ></iframe>
        ) : (
          <p>Unable to load document</p>
        )}
        <button onClick={closeModal} className={styles.closeModalButton}>
          Close
        </button>
      </Modal>
    </div>
  );
};

export default DashboardTable;
