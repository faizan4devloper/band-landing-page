import React, { useState, useEffect } from 'react';
import axios from 'axios';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { 
  faSync, 
  faFileAlt, 
  faEye, 
  faDownload, 
  faFilter,
  faRotateRight
} from '@fortawesome/free-solid-svg-icons';
import { motion, AnimatePresence } from 'framer-motion';
import Modal from 'react-modal';
import styles from './DashboardTable.module.css';

const DashboardTable = ({ userEmail }) => {
  const [documents, setDocuments] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [currentPage, setCurrentPage] = useState(0);
  const [pageSize] = useState(2);
  const [nextStartKey, setNextStartKey] = useState(null);
  const [selectedMetadata, setSelectedMetadata] = useState(null);
  const [isModalOpen, setIsModalOpen] = useState(false);

  const API_ENDPOINT = 'dummy'; // Replace with the actual API endpoint

  const parseResponse = (response) => {
    try {
      const responseData = typeof response === 'string' 
        ? JSON.parse(response) 
        : response;

      const parsedBody = typeof responseData.body === 'string'
        ? JSON.parse(responseData.body)
        : responseData.body;

      return parsedBody;
    } catch (parseError) {
      setError('Failed to parse response');
      return null;
    }
  };

  const fetchDocuments = async (isRefresh = false) => {
    setLoading(true);
    setError(null);
    try {
      const payload = {
        queryStringParameters: isRefresh 
          ? { 
              refresh: true, 
              page_size: pageSize 
            }
          : { 
              start_page: currentPage, 
              page_size: pageSize,
              ...(nextStartKey && { next_start_key: nextStartKey })
            }
      };

      const response = await axios.post(API_ENDPOINT, payload, {
        headers: {
          'Content-Type': 'application/json'
        }
      });

      const parsedBody = parseResponse(response.data);

      if (!parsedBody || !parsedBody.data) {
        throw new Error('No data in response');
      }

      const allTransactions = parsedBody.data.flatMap(page => 
        page.transactions.map(transaction => {
          try {
            return {
              Filename: transaction.Filename.S,
              Category: transaction.Category.S,
              DateTime: transaction.DateTime.S,
              TransactionID: transaction.TransactionID.S,
              Metadata: Object.fromEntries(
                Object.entries(transaction.Metadata.M).map(([key, value]) => [key, value.S])
              )
            };
          } catch (transformError) {
            return null;
          }
        }).filter(Boolean)
      );

      setDocuments(allTransactions);
      setNextStartKey(parsedBody?.next_start_key || null);
      setLoading(false);
    } catch (error) {
      setError(error.message || 'Failed to fetch documents');
      setDocuments([]);
      setLoading(false);
    }
  };

  useEffect(() => {
    fetchDocuments();
  }, [currentPage]);

  const renderDocumentRow = (doc) => {
    const displayFields = {
      'AccountStatement': [
        doc.Metadata['Account No.'] || 'N/A',
        doc.Metadata['Date'] || 'N/A'
      ],
      'CreditNote': [
        doc.Metadata['Credit Note'] || 'N/A',
        doc.Metadata['Date/Tax Point'] || 'N/A'
      ]
    };

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
          <button
            className={styles.seeMoreButton}
            onClick={() => handleMetadataClick(doc.Metadata)}
          >
            See More
          </button>
        </td>
        <td>{new Date(doc.DateTime).toLocaleString()}</td>
        <td>
          <div className={styles.actionButtons}>
            <button 
              className={styles.viewButton}
              title="View Document"
            >
              <FontAwesomeIcon icon={faEye} />
            </button>
            <button 
              className={styles.downloadButton}
              title="Download Document"
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
              <FontAwesomeIcon 
                icon={faSync} 
                spin 
                className={styles.loadingIcon}
              />
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
      setCurrentPage(prev => prev + 1);
    } else if (direction === 'prev' && currentPage > 0) {
      setCurrentPage(prev => Math.max(0, prev - 1));
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
            <FontAwesomeIcon icon={faRotateRight} />
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
              <th>Actions</th>
            </tr>
          </thead>
          <tbody>
            <AnimatePresence>
              {renderTableContent()}
            </AnimatePresence>
          </tbody>
        </table>
      </div>

      {/* Pagination */}
      <div className={styles.pagination}>
        <button 
          onClick={() => handlePageChange('prev')}
          disabled={currentPage === 0}
        >
          Previous
        </button>
        <span>Page {currentPage + 1}</span>
        <button 
          onClick={() => handlePageChange('next')}
          disabled={!nextStartKey}
        >
          Next
        </button>
      </div>

      {/* Modal for Metadata */}
      <Modal
        isOpen={isModalOpen}
        onRequestClose={closeModal}
        contentLabel="Metadata Details"
        className={styles.modal}
        overlayClassName={styles.overlay}
      >
        <h2>Metadata Details</h2>
        <pre>{JSON.stringify(selectedMetadata, null, 2)}</pre>
        <button onClick={closeModal} className={styles.closeModalButton}>Close</button>
      </Modal>
    </div>
  );
};

export default DashboardTable;



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
  background-color: #4CAF50;
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
  background-color: #007bff;
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
  background-color: #28a745;
  color: white;
  border: none;
  padding: 6px 12px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}

.seeMoreButton:hover {
  background-color: #218838;
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
