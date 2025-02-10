const handleDownloadDocument = async (category, filename) => {
  try {
    const presignedUrlResponse = await axios.post(
      'https://umo-indexer-presignedurl-v1',
      { filename }
    );

    const responseBody = typeof presignedUrlResponse.data.body === 'string'
      ? JSON.parse(presignedUrlResponse.data.body)
      : presignedUrlResponse.data.body;

    // Prefer CSV for download, fallback to PDF
    let presignedUrl = responseBody.csv_presigned_url || responseBody.pdf_presigned_url;
    let downloadFilename = filename;

    if (presignedUrl) {
      if (presignedUrl === responseBody.csv_presigned_url) {
        downloadFilename = filename.endsWith('.csv') ? filename : `${filename}.csv`;
      } else {
        downloadFilename = filename.endsWith('.pdf') ? filename : `${filename}.pdf`;
      }

      const link = document.createElement('a');
      link.href = presignedUrl;
      link.download = downloadFilename;
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
    } else {
      throw new Error('No download URL available');
    }
  } catch (error) {
    console.error('Download Error:', error);
    alert(`Failed to download document: ${error.message}`);
  }
};




import React, { useState, useEffect } from "react";
import axios from "axios";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faRotate } from "@fortawesome/free-solid-svg-icons";
import { motion } from "framer-motion";
import Modal from "react-modal";
import TableData from "./TableData"; // Import the new TableData component
import styles from "./DashboardTable.module.css";

// Set app element for accessibility
Modal.setAppElement("#root");

const DashboardTable = ({ userEmail }) => {
  const [documents, setDocuments] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [currentPage, setCurrentPage] = useState(0);
  const [pageSize] = useState(20);
  const [nextStartKey, setNextStartKey] = useState(null);
  const [selectedMetadata, setSelectedMetadata] = useState(null);
  const [isModalOpen, setIsModalOpen] = useState(false);
  const [selectedDocument, setSelectedDocument] = useState(null);
  const [isDocumentPreviewOpen, setIsDocumentPreviewOpen] = useState(false);
  const [isMetadataModalOpen, setIsMetadataModalOpen] = useState(false);
    const [isCopied, setIsCopied] = useState(false);


  const API_ENDPOINT = "doc-indexer"; // Replace with the actual API endpoint


const fetchDocuments = async (isRefresh = false, newPage = null) => {
  setLoading(true);
  setError(null);

  try {
    let startPage = newPage !== null ? newPage : currentPage + 1;
    let refresh = startPage <= 3 ? true : false; // First 3 pages refresh, others don't

    const payload = {
      action_type: "pagination",
      refresh: refresh,
      start_page: startPage,
      page_size: 20,
    };

    console.log("Sending Payload:", payload);

    const response = await axios.post(API_ENDPOINT, payload, {
      headers: { "Content-Type": "application/json" },
    });

    console.log("API Response:", response.data);

    const parsedBody = parseResponse(response.data);
    if (!parsedBody || !parsedBody.pages) {
      throw new Error("No pages found in response");
    }

    // Extract transactions from the correct page
    const pageKey = `page_${startPage}`;
    const transactions = parsedBody.pages[pageKey] || [];

    setDocuments(transactions);
    setCurrentPage(startPage);
  } catch (err) {
    setError(err.message || "Failed to fetch documents");
    setDocuments([]);
  } finally {
    setLoading(false);
  }
};

const handlePageChange = (direction) => {
  setCurrentPage((prev) => {
    let newPage = direction === "next" ? prev + 1 : Math.max(1, prev - 1);
    fetchDocuments(false, newPage);
    return newPage;
  });
};

const handleMetadataClick = (metadata) => {
  setSelectedMetadata(metadata);
  setIsMetadataModalOpen(true);
};


const handleRefresh = () => {
  setCurrentPage(1);
  fetchDocuments(true, 1); // Refresh with page 1
};

  useEffect(() => {
    fetchDocuments();
  }, []);
  
  useEffect(() => {
  console.log("Updated Documents:", documents);
}, [documents]);


  const parseResponse = (response) => {
    try {
      const responseData =
        typeof response === "string" ? JSON.parse(response) : response;
      return typeof responseData.body === "string"
        ? JSON.parse(responseData.body)
        : responseData.body;
    } catch {
      setError("Failed to parse response");
      return null;
    }
  };

const handleViewDocument = async (category, filename) => {
  try {
    const presignedUrlPayload = {
      filename: filename // Just pass the filename
    };

    const presignedUrlResponse = await axios.post(
      'https://umo-indexer-presignedurl-v1',
      presignedUrlPayload
    );

    // Parse the response body
    const responseBody = typeof presignedUrlResponse.data.body === 'string' 
      ? JSON.parse(presignedUrlResponse.data.body) 
      : presignedUrlResponse.data.body;

    // Prioritize PDF URL, fallback to CSV if PDF not available
    const presignedUrl = responseBody.pdf_presigned_url || responseBody.csv_presigned_url;
    
    if (presignedUrl) {
      setSelectedDocument({
        url: presignedUrl,
        filename: filename,
        metadata: documents.find(doc => doc.Filename.split('/').pop() === filename)?.Metadata
      });
      setIsDocumentPreviewOpen(true);
    } else {
      throw new Error('No presigned URL found');
    }
  } catch (error) {
    console.error('Detailed View Error:', error);
    alert(`Failed to view document: \${error.message}`);
  }
};

const handleDownloadDocument = async (category, filename) => {
  try {
    const presignedUrlPayload = {
      filename: filename // Just pass the filename
    };

    const presignedUrlResponse = await axios.post(
      'https://umo-indexer-presignedurl-v1', 
      presignedUrlPayload
    );

    // Parse the response body
    const responseBody = typeof presignedUrlResponse.data.body === 'string' 
      ? JSON.parse(presignedUrlResponse.data.body) 
      : presignedUrlResponse.data.body;

    // Determine download URL (prefer PDF, fallback to CSV)
    let presignedUrl;
    let downloadFilename = filename;

    if (responseBody.pdf_presigned_url) {
      presignedUrl = responseBody.pdf_presigned_url;
      // Ensure .pdf extension if not already present
      downloadFilename = filename.endsWith('.pdf') ? filename : `\${filename}.pdf`;
    } else if (responseBody.csv_presigned_url) {
      presignedUrl = responseBody.csv_presigned_url;
      // Ensure .csv extension if not already present
      downloadFilename = filename.endsWith('.csv') ? filename : `\${filename}.csv`;
    } else {
      throw new Error('No download URL available');
    }

    if (presignedUrl) {
      // Create a temporary anchor element to trigger download
      const link = document.createElement('a');
      link.href = presignedUrl;
      link.download = downloadFilename;
      
      // Append to body, click, and remove
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
    } else {
      throw new Error('Invalid download URL');
    }
  } catch (error) {
    console.error('Download Error:', error);
    alert(`Failed to download document: \${error.message}`);
  }

const handleCopy = () => {
    navigator.clipboard.writeText(JSON.stringify(selectedMetadata, null, 2));
    setIsCopied(true);
    
    // Reset the copied state after 2 seconds
    setTimeout(() => {
      setIsCopied(false);
    }, 3000);
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

      {/* Table Component */}
      <TableData
        documents={documents}
        loading={loading}
        currentPage={currentPage}
        nextStartKey={nextStartKey}
        handlePageChange={handlePageChange}
        handleRefresh={handleRefresh}
        handleViewDocument={handleViewDocument}
        handleDownloadDocument={handleDownloadDocument}
        handleMetadataClick={handleMetadataClick}
      />

      {/* Modal for Metadata */}
      <Modal
        isOpen={isDocumentPreviewOpen}
        onRequestClose={() => setIsDocumentPreviewOpen(false)}
        className={styles.modalContainer}
        overlayClassName={styles.overlay}
      >
        {selectedDocument && (
          <div className={styles.modalContent}>
            {/* PDF Preview Section */}
            <div className={styles.previewSection}>
              <div className={styles.previewWrapper}>
                <iframe
                  src={selectedDocument.url}
                  className={styles.previewIframe}
                  title={selectedDocument.filename}
                />
              </div>
            </div>

            {/* Metadata Section */}
            <div className={styles.metadataSection}>
              <div className={styles.metadataHeader}>
                <h2 className={styles.documentTitle}>
                  {selectedDocument.filename}
                </h2>
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
                <button
                  onClick={() => setIsDocumentPreviewOpen(false)}
                  className={styles.closeModalButton}
                >
                  Close
                </button>
              </div>
            </div>
          </div>
        )}
      </Modal>
      {/* Modal for Metadata */}
<Modal
      isOpen={isMetadataModalOpen}
      onRequestClose={() => setIsMetadataModalOpen(false)}
      className={styles.metadataModalContainer}
      overlayClassName={styles.overlay}
    >
      {selectedMetadata && (
        <div className={styles.metadataModalContent}>
          <div className={styles.metadataModalHeader}>
            <h2>Metadata Details</h2>
            <button 
              onClick={handleCopy}
              className={`${styles.copyButton} ${isCopied ? styles.copied : ''}`}
            >
              {isCopied ? 'Copied!' : 'Copy'}
            </button>
          </div>
          
          <div className={styles.metadataJsonContainer}>
            <pre className={styles.metadataJsonPre}>
              {JSON.stringify(selectedMetadata, null, 2)}
            </pre>
          </div>
          
          <div className={styles.metadataModalActions}>
            <button 
              onClick={() => setIsMetadataModalOpen(false)}
              className={styles.closeMetadataButton}
            >
              Close
            </button>
          </div>
        </div>
      )}
    </Modal>
    </div>
  );
};

export default DashboardTable;




import React from "react";
import { motion, AnimatePresence } from "framer-motion";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import {
  faSync,
  faEye,
  faDownload,
  faChevronLeft,
  faChevronRight,
  faFileLines
} from "@fortawesome/free-solid-svg-icons";
import styles from "./TableData.module.css";



const TableData = ({
  documents,
  loading,
  currentPage,
  nextStartKey,
  handlePageChange,
  handleRefresh,
  handleViewDocument,
  handleDownloadDocument,
  handleMetadataClick
}) => {
  const renderDocumentRow = (doc) => (
    <motion.tr
      key={doc.TransactionID}
      initial={{ opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
      transition={{ duration: 0.3 }}
      className={styles.documentRow}
    >
      <td>{doc.Filename.split("/").pop()}</td>
      <td>{doc.Metadata?.Category}</td>
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
            onClick={() =>
              handleViewDocument(doc.Category, doc.Filename.split("/").pop())
            }
          >
            <FontAwesomeIcon icon={faEye} />
          </button>
          <button
            className={styles.downloadButton}
            title="Download Document"
            onClick={() =>
              handleDownloadDocument(doc.Category, doc.Filename.split("/").pop())
            }
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
              <p>No documents found</p>
            </div>
          </td>
        </tr>
      );
    }

    return documents.map(renderDocumentRow);
  };

  const totalPages = Math.ceil(documents.length / 10); // Adjust for total pages based on document count

  const renderPaginationButtons = () => {
    const pageNumbers = [];
    for (let i = 1; i <= totalPages; i++) {
      pageNumbers.push(i);
    }

    return pageNumbers.map((pageNumber) => (
      <button
        key={pageNumber}
        className={`${styles.paginationButton} ${
          currentPage === pageNumber - 1 ? styles.active : ""
        }`}
        onClick={() => handlePageChange(pageNumber - 1)}
      >
        {pageNumber}
      </button>
    ));
  };

  return (
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

      {/* Pagination */}
      <div className={styles.pagination}>
        <button
          className={`${styles.paginationButton} ${
            currentPage === 0 ? styles.disabled : ""
          }`}
          onClick={() => handlePageChange("prev")}
          disabled={currentPage === 0}
        >
          <FontAwesomeIcon icon={faChevronLeft} className={styles.paginationIcon} />
          Prev
        </button>

        <div className={styles.paginationNumbers}>
          {renderPaginationButtons()}
        </div>

        <button
          className={`${styles.paginationButton} ${
            currentPage === totalPages - 1 ? styles.disabled : ""
          }`}
          onClick={() => handlePageChange("next")}
          disabled={currentPage === totalPages - 1}
        >
          Next
          <FontAwesomeIcon icon={faChevronRight} className={styles.paginationIcon} />
        </button>
      </div>
    </div>
  );
};

export default TableData;

