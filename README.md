import React, { useState, useEffect } from "react";
import axios from "axios";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faEye } from "@fortawesome/free-solid-svg-icons";
import Modal from "react-modal";
import { motion, AnimatePresence } from "framer-motion";
import styles from "./DashboardTable.module.css";

// Set app element for accessibility
Modal.setAppElement("#root");

const DashboardTable = ({ userEmail }) => {
  const [documents, setDocuments] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [isModalOpen, setIsModalOpen] = useState(false);
  const [selectedDocument, setSelectedDocument] = useState(null);
  const [previewUrl, setPreviewUrl] = useState("");

  const API_ENDPOINT = "https:dummy"; // Replace with your actual API endpoint

  // Fetch documents
  const fetchDocuments = async () => {
    setLoading(true);
    setError(null);
    try {
      const response = await axios.post(API_ENDPOINT, {
        queryStringParameters: { page_size: 5 },
      });

      const parsedData = JSON.parse(response.data.body);
      setDocuments(parsedData.data.flatMap((page) => page.transactions));
    } catch (err) {
      setError("Failed to fetch documents.");
    } finally {
      setLoading(false);
    }
  };

  useEffect(() => {
    fetchDocuments();
  }, []);

  // Handle opening the modal
  const openModal = async (doc) => {
    setSelectedDocument(doc);

    try {
      // Request presigned URL for preview
      const payload = {
        category: doc.Category.S,
        filename: doc.Metadata.M.Filename.S,
      };

      const response = await axios.post(
        "https://dummypresignedurl", // Replace with your actual presigned URL endpoint
        payload
      );

      setPreviewUrl(response.data.presignedUrl);
      setIsModalOpen(true);
    } catch (err) {
      console.error("Failed to fetch presigned URL:", err);
    }
  };

  // Close the modal
  const closeModal = () => {
    setIsModalOpen(false);
    setSelectedDocument(null);
    setPreviewUrl("");
  };

  if (loading) return <p>Loading...</p>;
  if (error) return <p>{error}</p>;

  return (
    <div>
      <table className={styles.table}>
        <thead>
          <tr>
            <th>Filename</th>
            <th>Status</th>
            <th>Category</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          {documents.map((doc, index) => (
            <tr key={index}>
              <td>{doc.Metadata.M.Filename.S}</td>
              <td>{doc.Status.S}</td>
              <td>{doc.Category.S}</td>
              <td>
                <FontAwesomeIcon
                  icon={faEye}
                  className={styles.icon}
                  onClick={() => openModal(doc)}
                />
              </td>
            </tr>
          ))}
        </tbody>
      </table>

      {/* Modal */}
      <AnimatePresence>
        {isModalOpen && (
          <Modal
            isOpen={isModalOpen}
            onRequestClose={closeModal}
            className={styles.modal}
            overlayClassName={styles.overlay}
          >
            <motion.div
              initial={{ opacity: 0, y: 50 }}
              animate={{ opacity: 1, y: 0 }}
              exit={{ opacity: 0, y: 50 }}
              className={styles.modalContent}
            >
              {/* Left: Document Preview */}
              <div className={styles.preview}>
                {previewUrl ? (
                  <iframe
                    src={previewUrl}
                    title="Document Preview"
                    className={styles.iframe}
                  />
                ) : (
                  <p>Loading preview...</p>
                )}
              </div>

              {/* Right: Metadata */}
              <div className={styles.metadata}>
                <h3>Metadata</h3>
                {selectedDocument &&
                  Object.entries(selectedDocument.Metadata.M).map(
                    ([key, value]) => (
                      <p key={key}>
                        <strong>{key}:</strong> {value.S || "N/A"}
                      </p>
                    )
                  )}
              </div>

              <button onClick={closeModal} className={styles.closeButton}>
                Close
              </button>
            </motion.div>
          </Modal>
        )}
      </AnimatePresence>
    </div>
  );
};

export default DashboardTable;




/* DashboardTable.module.css */
.table {
  width: 100%;
  border-collapse: collapse;
}

.table th,
.table td {
  padding: 10px;
  border: 1px solid #ddd;
  text-align: left;
}

.icon {
  cursor: pointer;
  color: #007bff;
  transition: transform 0.3s ease;
}

.icon:hover {
  transform: scale(1.2);
}

.modal {
  display: flex;
  flex-direction: column;
  background: white;
  max-width: 80%;
  margin: auto;
  padding: 20px;
  border-radius: 8px;
  outline: none;
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
}

.modalContent {
  display: flex;
  gap: 20px;
}

.preview {
  flex: 1;
  border-right: 1px solid #ddd;
  padding-right: 20px;
}

.iframe {
  width: 100%;
  height: 400px;
}

.metadata {
  flex: 1;
}

.closeButton {
  margin-top: 20px;
  padding: 10px 20px;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.closeButton:hover {
  background: #0056b3;
}







i want the after i click the icon then open beutiful modal and in that display the meta data right side and left side document preview this is my api data:- {
    "statusCode": 200,
    "headers": {
        "Content-Type": "application/json"
    },
    "body": "{\"data\": [{\"page_number\": 1, \"transactions\": [{\"Status\": {\"S\": \"Success\"}, \"Filename\": {\"S\": \"processed/Statement_data_1_1.pdf\"}, \"Metadata\": {\"M\": {\"Account No.\": {\"S\": \"GB014384\"}, \"Filename\": {\"S\": \"Statement_data_1.pdf\"}, \"Category\": {\"S\": \"Account Statement\"}, \"Email\": {\"S\": \"ukcreditcontrol@umusic.com\"}, \"Date\": {\"S\": \"01.08.2024\"}}}, \"TransactionID\": {\"S\": \"7dfb21f1-d4c8-4c39-9fa3-d1ab4f9623cd\"}, \"Category\": {\"S\": \"AccountStatement\"}, \"DateTime\": {\"S\": \"2025-01-08T10:31:08.749612\"}}, {\"Status\": {\"S\": \"Success\"}, \"Filename\": {\"S\": \"processed/Statement_data_10.pdf\"}, \"Metadata\": {\"M\": {\"Account No.\": {\"S\": \"GB012087\"}, \"Filename\": {\"S\": \"Statement_data_10.pdf\"}, \"Category\": {\"S\": \"Account Statement\"}, \"Email\": {\"S\": \"ukcreditcontrol@umusic.com\"}, \"Date\": {\"S\": \"01.08.2024\"}}}, \"TransactionID\": {\"S\": \"6819edf0-6643-4ce8-b0ff-22dd93ba2641\"}, \"Category\": {\"S\": \"AccountStatement\"}, \"DateTime\": {\"S\": \"2024-12-17T11:35:21.532463\"}}]}, {\"page_number\": 2, \"transactions\": [{\"Status\": {\"S\": \"Success\"}, \"Filename\": {\"S\": \"Statement_data_1.pdf\"}, \"Metadata\": {\"M\": {\"Account No.\": {\"S\": \"GB014384\"}, \"Filename\": {\"S\": \"Statement_data_1.pdf\"}, \"Category\": {\"S\": \"Account Statement\"}, \"Email\": {\"S\": \"ukcreditcontrol@umusic.com\"}, \"Date\": {\"S\": \"01.08.2024\"}}}, \"TransactionID\": {\"S\": \"4c942959-79a4-4952-9125-cadb42ed4c25\"}, \"Category\": {\"S\": \"AccountStatement\"}, \"DateTime\": {\"S\": \"2024-12-17T06:05:52.610414\"}}, {\"Status\": {\"S\": \"Success\"}, \"Filename\": {\"S\": \"processed/Statement_data_8014.pdf\"}, \"Metadata\": {\"M\": {\"Account No.\": {\"S\": \"GB014384\"}, \"Filename\": {\"S\": \"Statement_data_8014.pdf\"}, \"Category\": {\"S\": \"Account Statement\"}, \"Date\": {\"S\": \"01.08.2024\"}}}, \"TransactionID\": {\"S\": \"ff958ed1-fd96-40b0-b63e-1b8f84985942\"}, \"Category\": {\"S\": \"AccountStatement\"}, \"DateTime\": {\"S\": \"2025-01-19T17:48:17.243534\"}}]}, {\"page_number\": 3, \"transactions\": [{\"Status\": {\"S\": \"Success\"}, \"Filename\": {\"S\": \"processed/OPD_UK_Credit_Note_data_41.pdf\"}, \"Metadata\": {\"M\": {\"Category\": {\"S\": \"Credit Note\"}, \"Credit Note\": {\"S\": \"89013102\"}, \"Date/Tax Point\": {\"S\": \"31/07/24\"}, \"Return Number\": {\"S\": \"2024073133063\"}, \"Delivery Account\": {\"S\": \"16992\"}, \"Filename\": {\"S\": \"OPD_UK_Credit_Note_data_41.pdf\"}, \"Statement Account\": {\"S\": \"16992\"}}}, \"TransactionID\": {\"S\": \"8f36941e-e83d-461b-ae86-7bf0a3dbca66\"}, \"Category\": {\"S\": \"CreditNote\"}, \"DateTime\": {\"S\": \"2025-01-14T10:01:02.734063\"}}, {\"Status\": {\"S\": \"Failure\"}, \"Filename\": {\"S\": \"Statement_data_802.pdf\"}, \"Metadata\": {\"M\": {}}, \"TransactionID\": {\"S\": \"1b75a402-584c-42a3-a126-b6d06e9b9d25\"}, \"Category\": {\"S\": \"AccountStatement\"}, \"DateTime\": {\"S\": \"2025-01-19T16:59:49.897961\"}}]}], \"next_start_key\": {\"TransactionID\": {\"S\": \"1b75a402-584c-42a3-a126-b6d06e9b9d25\"}, \"Filename\": {\"S\": \"Statement_data_802.pdf\"}}}"
}

i want to show the preview of the pdf in that currently i;m unable to preview from this data i have created the presign url for preview the doc:- Presigned-URL:
 
https:dummypresignedurl
 
Sample Payload:
{
  "category": "CreditNote",
  "filename": "UK-C-013047-013047-89161694.pdf"
}


and this is my code:- 

import React, { useState, useRef } from 'react';
  import axios from 'axios';
  import styles from './Dashboard.module.css';
  import Sidebar from './Sidebar';
  import DashboardTable from './DashboardTable';
  import Summary from '../SummaryContent/Summary';
  
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
            'https:dummy', // Replace 'dummy' with your actual API endpoint
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
          <Summary/>
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

  const API_ENDPOINT = 'https:dummy'; // Replace with the actual API endpoint

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

import React, { useState, useEffect } from 'react';
import styles from './Dashboard.module.css';

function PDFPreview({ pdfUrl, fileType }) {
  const [validPdfUrl, setValidPdfUrl] = useState(null);

  useEffect(() => {
    // Check if the PDF URL is a presigned URL or needs processing
    if (pdfUrl) {
      // If it's a direct S3 presigned URL, use it directly
      if (pdfUrl.includes('https://') && (pdfUrl.endsWith('.pdf') || pdfUrl.includes('?'))) {
        setValidPdfUrl(pdfUrl);
      } else {
        // If it's not a direct URL, you might need to construct the URL
        // This depends on how your backend provides the URL
        const constructedUrl = constructPdfUrl(pdfUrl, fileType);
        setValidPdfUrl(constructedUrl);
      }
    }
  }, [pdfUrl, fileType]);

  // Helper function to construct PDF URL if needed
  const constructPdfUrl = (url, fileType) => {
    // Implement your URL construction logic here
    // This could involve calling an API endpoint to get the presigned URL
    // Example:
    // return `https://your-api.com/pdf-preview?key=${url}&type=${fileType}`;
    return url;
  };

  if (!validPdfUrl) return null;

  return (
    <div className={styles.pdfPreviewSection}>
      <h3>PDF Preview</h3>
      <div className={styles.pdfPreviewContainer}>
        <iframe
          src={validPdfUrl}
          width="100%"
          height="600px"
          title="PDF Preview"
          style={{
            border: '1px solid #ccc',
            borderRadius: '8px',
            boxShadow: '0 4px 6px rgba(0,0,0,0.1)'
          }}
        />
      </div>
    </div>
  );
}

export default PDFPreview;
  
