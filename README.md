i want add new component called summary in that:- 
1. tile 1 pagination will fetch 3 pagesn the data and time
2. tile 2 needs total and successful transactions for current date
3. tile 3 needs total and successful transaction counts for current date
4. tile 4 needs total and successful transaction counts forlast one monts

i have seprate apis for tiles

import React, { useState, useRef } from 'react';
import axios from 'axios';
import styles from './Dashboard.module.css';
import Sidebar from './Sidebar';
import DashboardTable from './DashboardTable';

// Define file type mappings
const FILE_TYPE_MAPPINGS = {
  'aimlusecasesv1': 'AI',
  'umo-privilegestatement': 'PS',
  'umo-invoice': 'CL',
  'umo-creditnote': 'MD',
  'umo-returnauthorisation': 'IN',
  'umo-accountstatement': 'OT',
};

function Dashboard({ userEmail }) {
  const [selectedCategory, setSelectedCategory] = useState('');
  const [selectedFiles, setSelectedFiles] = useState(null);
  const [uploadProgress, setUploadProgress] = useState(0);
  const [uploadStatus, setUploadStatus] = useState('');
  const fileInputRef = useRef(null);

  // Handle file selection
  const handleFileChange = (fileList) => {
    // If fileList is null, reset selected files
    if (!fileList) {
      setSelectedFiles(null);
      return;
    }

    // Convert fileList to array if it's not null
    setSelectedFiles(fileList.length > 0 ? Array.from(fileList) : null);
  };

  // Trigger file input (optional, can be used if needed)
  const triggerFileInput = () => {
    if (fileInputRef.current) {
      fileInputRef.current.click();
    }
  };

  // Handle file upload
const handleUpload = async () => {
  if (!selectedFiles || selectedFiles.length === 0 || !selectedCategory) {
    alert('Please select at least one file and a category.');
    return;
  }

  try {
    setUploadProgress(0);
    setUploadStatus('Uploading...');

    const uploadedFiles = [];
    const failedFiles = [];

    for (let file of selectedFiles) {
      try {
        const fileTypeCode = FILE_TYPE_MAPPINGS[selectedCategory] || 'OT';
        const payload = {
          payload: {
            filename: file.name,
            filetype: fileTypeCode,
            fileSize: file.size,
            fileType: file.type,
          },
        };

        // Step 1: Request presigned URL for the current file
        const urlResponse = await axios.post(
          'dummy', // Replace 'dummy' with your actual API endpoint
          payload,
          { headers: { 'Content-Type': 'application/json' } }
        );
        
        console.log('URL:', urlResponse)

        const { presignedUrl, key, recNum, bucket } = urlResponse.data;

        // Step 2: Upload the current file to S3 using the presigned URL
        await axios.put(presignedUrl, file, {
          headers: { 'Content-Type': file.type },
          onUploadProgress: (progressEvent) => {
            const percentCompleted = Math.round(
              (progressEvent.loaded * 100) / progressEvent.total
            );
            setUploadProgress(percentCompleted);
          },
        });

        uploadedFiles.push({ 
          fileName: file.name, 
          recordNumber: recNum, 
          s3Key: key, 
          bucket 
        });
      } catch (error) {
        console.error(`Failed to upload file: ${file.name}`, error);
        failedFiles.push(file.name);
      }
    }

    // Update upload status
    if (failedFiles.length === 0) {
      setUploadStatus('All files uploaded successfully!');
    } else if (failedFiles.length === selectedFiles.length) {
      setUploadStatus('All uploads failed.');
    } else {
      setUploadStatus(
        `Partial success: ${uploadedFiles.length} files uploaded, ${failedFiles.length} failed.`
      );
    }

    // Reset state after upload
    setSelectedFiles(null);
    setSelectedCategory('');
    setUploadProgress(0);

  } catch (error) {
    console.error('Error during upload', error);
    setUploadStatus('Upload failed: An unexpected error occurred.');
  }
};


  return (
    <div className={styles.dashboardLayout}>
      <Sidebar
        selectedCategory={selectedCategory}
        setSelectedCategory={setSelectedCategory}
        selectedFiles={selectedFiles}
        handleFileChange={handleFileChange}
        handleUpload={handleUpload}
        uploadProgress={uploadProgress}
        uploadStatus={uploadStatus}
        fileTypeMappings={FILE_TYPE_MAPPINGS}
        fileInputRef={fileInputRef}
        triggerFileInput={triggerFileInput}
      />

      <div className={styles.mainContent}>
        <DashboardTable userEmail={userEmail} />
      </div>
    </div>
  );
}

export default Dashboard;


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

// Set app element for accessibility
Modal.setAppElement('#root');

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
  
  // console.log('Parsed Data:', parseResponse)

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
        isOpen={isModalOpen}
        onRequestClose={closeModal}
        contentLabel="Metadata Details"
        className={styles.modal}
        overlayClassName={styles.overlay}
      >
        <h2>Metadata Details</h2>
        <pre>{JSON.stringify(selectedMetadata, null, 2)}</pre>
        <button onClick={closeModal} className={styles.closeModalButton}>
          Close
        </button>
      </Modal>
    </div>
  );
};

export default DashboardTable;

