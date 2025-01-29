import axios from 'axios';

const API_ENDPOINT = 'https://dummy'; // Replace with actual API

export const fetchDocuments = async (currentPage, pageSize, nextStartKey, isRefresh) => {
  try {
    const payload = {
      queryStringParameters: isRefresh
        ? { refresh: true, page_size: pageSize }
        : { start_page: currentPage, page_size: pageSize, ...(nextStartKey && { next_start_key: nextStartKey }) },
    };

    const response = await axios.post(API_ENDPOINT, payload, {
      headers: { 'Content-Type': 'application/json' },
    });

    return parseResponse(response.data);
  } catch (error) {
    throw new Error(error.message || 'Failed to fetch documents');
  }
};

export const getPresignedUrl = async (category, filename) => {
  try {
    const response = await axios.post('https://dummy', { category, filename });
    const responseBody = JSON.parse(response.data.body);
    return responseBody.presigned_url;
  } catch (error) {
    throw new Error('Failed to fetch presigned URL');
  }
};

const parseResponse = (response) => {
  try {
    const responseData = typeof response === 'string' ? JSON.parse(response) : response;
    return typeof responseData.body === 'string' ? JSON.parse(responseData.body) : responseData.body;
  } catch {
    throw new Error('Failed to parse response');
  }
};









import React from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faEye, faDownload } from '@fortawesome/free-solid-svg-icons';
import { motion } from 'framer-motion';
import styles from './DashboardTable.module.css';

const DocumentRow = ({ doc, handleViewDocument, handleDownloadDocument, handleMetadataClick }) => {
  return (
    <motion.tr
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
};

export default DocumentRow;





import React from 'react';
import { AnimatePresence } from 'framer-motion';
import DocumentRow from './DocumentRow';
import styles from './DashboardTable.module.css';

const DocumentsTable = ({ documents, loading, error, handleViewDocument, handleDownloadDocument, handleMetadataClick }) => {
  if (loading) {
    return (
      <tr>
        <td colSpan="6" className={styles.loadingRow}>
          <div className={styles.loadingContent}>Loading documents...</div>
        </td>
      </tr>
    );
  }

  if (documents.length === 0) {
    return (
      <tr>
        <td colSpan="6" className={styles.emptyRow}>
          <div className={styles.emptyContent}>No documents found</div>
        </td>
      </tr>
    );
  }

  return (
    <tbody>
      <AnimatePresence>
        {documents.map((doc) => (
          <DocumentRow
            key={doc.TransactionID}
            doc={doc}
            handleViewDocument={handleViewDocument}
            handleDownloadDocument={handleDownloadDocument}
            handleMetadataClick={handleMetadataClick}
          />
        ))}
      </AnimatePresence>
    </tbody>
  );
};

export default DocumentsTable;





import React from 'react';
import Modal from 'react-modal';
import styles from './DashboardTable.module.css';

const DocumentPreviewModal = ({ isOpen, onClose, selectedDocument }) => {
  return (
    <Modal isOpen={isOpen} onRequestClose={onClose} className={styles.modalContainer} overlayClassName={styles.overlay}>
      {selectedDocument && (
        <div className={styles.modalContent}>
          <div className={styles.previewSection}>
            <iframe src={selectedDocument.url} className={styles.previewIframe} title={selectedDocument.filename} />
          </div>
          <div className={styles.metadataSection}>
            <h2>{selectedDocument.filename}</h2>
            <pre className={styles.metadataJson}>
              {selectedDocument.metadata ? JSON.stringify(selectedDocument.metadata, null, 2) : 'No metadata available'}
            </pre>
            <button onClick={onClose} className={styles.closeModalButton}>
              Close Preview
            </button>
          </div>
        </div>
      )}
    </Modal>
  );
};

export default DocumentPreviewModal;








import React, { useState, useEffect } from 'react';
import { fetchDocuments, getPresignedUrl } from './api';
import DocumentsTable from './DocumentsTable';
import DocumentPreviewModal from './DocumentPreviewModal';
import styles from './DashboardTable.module.css';

const DashboardTable = () => {
  const [documents, setDocuments] = useState([]);
  const [loading, setLoading] = useState(false);
  const [currentPage, setCurrentPage] = useState(0);
  const [nextStartKey, setNextStartKey] = useState(null);
  const [selectedDocument, setSelectedDocument] = useState(null);
  const [isPreviewOpen, setIsPreviewOpen] = useState(false);

  useEffect(() => {
    setLoading(true);
    fetchDocuments(currentPage, 2, nextStartKey, false)
      .then((data) => {
        setDocuments(data.data);
        setNextStartKey(data.next_start_key || null);
      })
      .catch((error) => console.error(error))
      .finally(() => setLoading(false));
  }, [currentPage]);

  const handleViewDocument = async (category, filename) => {
    const url = await getPresignedUrl(category, filename);
    setSelectedDocument({ url, filename });
    setIsPreviewOpen(true);
  };

  return (
    <div className={styles.documentTableContainer}>
      <h3>Dashboard</h3>
      <DocumentsTable
        documents={documents}
        loading={loading}
        handleViewDocument={handleViewDocument}
      />
      <DocumentPreviewModal isOpen={isPreviewOpen} onClose={() => setIsPreviewOpen(false)} selectedDocument={selectedDocument} />
    </div>
  );
};

export default DashboardTable;
