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
import styles from './DashboardTable.module.css';

const DashboardTable = ({ userEmail }) => {
  const [documents, setDocuments] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [currentPage, setCurrentPage] = useState(0);
  const [pageSize] = useState(2);
  const [nextStartKey, setNextStartKey] = useState(null);

  const API_ENDPOINT = “dummy”;

  const parseResponse = (response) => {
    try {
      // If response is a string, parse it
      const responseData = typeof response === 'string' 
        ? JSON.parse(response) 
        : response;

      // If body is a string, parse it
      const parsedBody = typeof responseData.body === 'string'
        ? JSON.parse(responseData.body)
        : responseData.body;

      console.log('Raw Response:', response);
      console.log('Parsed Response Data:', responseData);
      console.log('Parsed Body:', parsedBody);

      return parsedBody;
    } catch (parseError) {
      console.error('Parsing Error:', parseError);
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

      // Parse the response
      const parsedBody = parseResponse(response.data);

      if (!parsedBody || !parsedBody.data) {
        throw new Error('No data in response');
      }

      // Flatten transactions from all pages
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
            console.error('Transaction Transform Error:', transformError);
            return null;
          }
        }).filter(Boolean)
      );

      console.log('Processed Transactions:', allTransactions);

      setDocuments(allTransactions);
      
      // Update next start key if exists
      setNextStartKey(parsedBody?.next_start_key || null);
      
      setLoading(false);
    } catch (error) {
      console.error('Fetch Error:', error);
      setError(error.message || 'Failed to fetch documents');
      setDocuments([]);
      setLoading(false);
    }
  };

  useEffect(() => {
    fetchDocuments();
  }, [currentPage]);

  const renderDocumentRow = (doc) => {
    // Determine display fields based on category
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
        <td>{displayFields[doc.Category]?.[0] || 'N/A'}</td>
        <td>{displayFields[doc.Category]?.[1] || 'N/A'}</td>
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
        <h3>Document Management</h3>
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
              <th>Reference</th>
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
    </div>
  );
};

export default DashboardTable;


.dashboardTableContainer {
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 20px;
}

.tableHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.refreshButton {
  background-color: #4CAF50;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
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
}

.documentTable th,
.documentTable td {
  border: 1px solid #ddd;
  padding: 12px;
  text-align: left;
}

.documentTable th {
  background-color: #f2f2f2;
  font-weight: bold;
}

.statusBadge {
  padding: 4px 8px;
  border-radius: 4px;
  font-size: 0.8em;
}

.processedStatus {
  background-color: #e7f5e7;
  color: #2e7d32;
}

.pendingStatus {
  background-color: #fff3e0;
  color: #ed6c02;
}

.errorStatus {
  background-color: #ffebee;
  color: #d32f2f;
}

.viewButton {
  background-color: #2196F3;
  color: white;
  border: none;
  padding: 6px 12px;
  border-radius: 4px;
  cursor: pointer;
}

.paginationContainer {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.pageButton {
  margin: 0 5px;
  padding: 6px 12px;
  border: 1px solid #ddd;
  background-color: white;
  cursor: pointer;
}

.activePage {
  background-color: #4CAF50;
  color: white;
  border: 1px solid #4CAF50;
}

.noDocumentsMessage {
  text-align: center;
  color: #888;
  padding: 20px;
}

.loadingContainer {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
}
.refreshButton {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.refreshIcon {
  margin-right: 5px;
}

.loadingContainer {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 20px;
  text-align: center;
}

.loadingIcon {
  font-size: 2rem;
  margin-bottom: 10px;
  color: #4CAF50;
}

.loadingRow, .emptyRow {
  text-align: center;
}

.loadingContent, .emptyContent {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 50px 0;
}

.loadingIcon, .emptyIcon {
  font-size: 3rem;
  margin-bottom: 15px;
  color: #4CAF50;
}

.emptyIcon {
  color: #888;
}

.loadingContent p, .emptyContent p {
  color: #666;
  font-size: 1rem;
}

/* Additional styles to prevent layout shift */
/*.documentTable {*/
/*  width: 100%;*/
/*  table-layout: fixed;*/
/*}*/

.documentTable td {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.documentTableContainer {
  background-color: #ffffff;
  border-radius: 12px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  padding: 20px;
}

.tableHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.headerActions {
  display: flex;
  align-items: center;
  gap: 15px;
}

.categoryFilter {
  display: flex;
  align-items: center;
  gap: 10px;
  background-color: #f4f4f4;
  padding: 8px 12px;
  border-radius: 8px;
}

.categorySelect {
  border: none;
  background: transparent;
  outline: none;
}

