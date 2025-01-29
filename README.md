import React, { useState } from 'react';
import { motion, AnimatePresence } from 'framer-motion';
import Modal from 'react-modal';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faSync, faFileAlt, faEye, faDownload, faRotate } from '@fortawesome/free-solid-svg-icons';
import styles from './DashboardTable.module.css';

Modal.setAppElement('#root');

const DocumentTableContainer = ({
  documents,
  loading,
  error,
  currentPage,
  nextStartKey,
  onPageChange,
  onRefresh,
  onViewDocument,
  onDownloadDocument,
}) => {
  const [selectedDocument, setSelectedDocument] = useState(null);
  const [isDocumentPreviewOpen, setIsDocumentPreviewOpen] = useState(false);

  const handleMetadataClick = (metadata) => {
    setSelectedDocument(prev => ({ ...prev, metadata }));
  };

  const handleClosePreview = () => {
    setIsDocumentPreviewOpen(false);
    setSelectedDocument(null);
  };

  const renderDocumentRow = (doc) => (
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
        <button className={styles.seeMoreButton} onClick={() => handleMetadataClick(doc.Metadata)}>
          See More
        </button>
      </td>
      <td>{new Date(doc.DateTime).toLocaleString()}</td>
      <td>
        <div className={styles.actionButtons}>
          <button
            className={styles.viewButton}
            title="View Document"
            onClick={() => handleViewDocument(doc.Category, doc.Filename.split('/').pop())}
          >
            <FontAwesomeIcon icon={faEye} />
          </button>
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
  );

  const renderTableContent = () => {
    if (loading) {
      return (
        <tr>
          <td colSpan="6" className={styles.loadingRow}>
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

  return (
    <div className={styles.documentTableContainer}>
      <div className={styles.tableHeader}>
        <h3>Dashboard</h3>
        <div className={styles.headerActions}>
          <motion.button
            onClick={onRefresh}
            className={styles.refreshButton}
            whileHover={{ scale: 1.05 }}
            whileTap={{ scale: 0.95 }}
          >
            <FontAwesomeIcon icon={faRotate} /> Refresh
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
          <tbody>
            <AnimatePresence>{renderTableContent()}</AnimatePresence>
          </tbody>
        </table>
      </div>

      <div className={styles.pagination}>
        <button onClick={() => onPageChange('prev')} disabled={currentPage === 0}>
          Previous
        </button>
        <span>Page {currentPage + 1}</span>
        <button onClick={() => onPageChange('next')} disabled={!nextStartKey}>
          Next
        </button>
      </div>

      <Modal
        isOpen={isDocumentPreviewOpen}
        onRequestClose={handleClosePreview}
        className={styles.modalContainer}
        overlayClassName={styles.overlay}
      >
        {selectedDocument && (
          <div className={styles.modalContent}>
            <div className={styles.previewSection}>
              <div className={styles.previewWrapper}>
                <iframe src={selectedDocument.url} className={styles.previewIframe} title={selectedDocument.filename} />
              </div>
            </div>
            <div className={styles.metadataSection}>
              <div className={styles.metadataHeader}>
                <h2 className={styles.documentTitle}>{selectedDocument.filename}</h2>
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
                <button onClick={handleClosePreview} className={styles.closeModalButton}>
                  Close Preview
                </button>
              </div>
            </div>
          </div>
        )}
      </Modal>
    </div>
  );
};

export default DocumentTableContainer;









import React, { useState, useEffect } from 'react';
import axios from 'axios';
import DocumentTableContainer from './DocumentTableContainer';

const DashboardTable = ({ userEmail }) => {
  const [documents, setDocuments] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [currentPage, setCurrentPage] = useState(0);
  const [pageSize] = useState(2);
  const [nextStartKey, setNextStartKey] = useState(null);

  const API_ENDPOINT = 'dummy'; // Replace with actual API endpoint

  const fetchDocuments = async (isRefresh = false) => {
    setLoading(true);
    setError(null);
    try {
      const payload = {
        queryStringParameters: isRefresh
          ? { refresh: true, page_size: pageSize }
          : {
              start_page: currentPage,
              page_size: pageSize,
              ...(nextStartKey && { next_start_key: nextStartKey })
            }
      };

      const response = await axios.post(API_ENDPOINT, payload, {
        headers: { 'Content-Type': 'application/json' }
      });

      const parsedBody = parseResponse(response.data);
      const allTransactions = parsedBody.data.flatMap((page) =>
        page.transactions.map((transaction) => ({
          Filename: transaction.Filename?.S || 'Unknown',
          Category: transaction.Category?.S || 'Unknown',
          DateTime: transaction.DateTime?.S || new Date().toISOString(),
          TransactionID: transaction.TransactionID?.S || 'Unknown',
          Metadata: Object.fromEntries(
            Object.entries(transaction.Metadata?.M || {}).map(([key, value]) => [key, value.S || 'N/A'])
          )
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

  const parseResponse = (response) => {
    try {
      const responseData = typeof response === 'string' ? JSON.parse(response) : response;
      return typeof responseData.body === 'string' ? JSON.parse(responseData.body) : responseData.body;
    } catch {
      setError('Failed to parse response');
      return null;
    }
  };

  const handleViewDocument = async (category, filename) => {
    try {
      const response = await axios.post('dummy', { category, filename });
      return JSON.parse(response.data.body).presigned_url;
    } catch (error) {
      throw new Error('Failed to get document URL');
    }
  };

  const handleDownloadDocument = async (category, filename) => {
    try {
      const response = await axios.post('dummy', { category, filename });
      return JSON.parse(response.data.body).presigned_url;
    } catch (error) {
      throw new Error('Failed to get download URL');
    }
  };

  const handlePageChange = (direction) => {
    if (direction === 'next' && documents.length > 0) {
      setCurrentPage((prev) => prev + 1);
    } else if (direction === 'prev' && currentPage > 0) {
      setCurrentPage((prev) => Math.max(0, prev - 1));
    }
  };

  useEffect(() => {
    fetchDocuments();
  }, [currentPage]);

  return (
    <DocumentTableContainer
      documents={documents}
      loading={loading}
      error={error}
      currentPage={currentPage}
      nextStartKey={nextStartKey}
      onPageChange={handlePageChange}
      onRefresh={() => fetchDocuments(true)}
      onViewDocument={handleViewDocument}
      onDownloadDocument={handleDownloadDocument}
    />
  );
};

export default DashboardTable;
