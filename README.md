why category th is empty

Updated Documents: 
(2) [{…}, {…}]
0
: 
DateTime
: 
"2025-02-03T06:34:38.575856"
Filename
: 
"UK-C-011668-018178-89161714.pdf"
Metadata
: 
Category
: 
"Credit Note"


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

  const API_ENDPOINT = "https://gt94pee06i.execute-api.us-east-1.amazonaws.com/doc-indexer-dev/doc-indexer"; // Replace with the actual API endpoint


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
      category: category,
      filename: filename
    };

    // Make a POST request to get the presigned URL
    const presignedUrlResponse = await axios.post(
      'https://mapz0hzanl.execute-api.us-east-1.amazonaws.com/dev/umo-indexer-presignedurl-v1', 
      presignedUrlPayload
    );
    
    console.log('PresignUpload Raw Response:', presignedUrlResponse);

    // Parse the body which is a string
    const responseBody = JSON.parse(presignedUrlResponse.data.body);
    
    // Extract the presigned URL 
    const presignedUrl = responseBody.presigned_url;
    
    console.log('Preview Presignurl:', presignedUrl);

    // Open the PDF in a new tab
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
    console.error('Error fetching presigned URL:', error);
    alert('Failed to view document');
  }
};

const handleDownloadDocument = async (category, filename) => {
  try {
    const presignedUrlPayload = {
      category: category,
      filename: filename
    };

    // Make a POST request to get the presigned URL
    const presignedUrlResponse = await axios.post(
      'https://umo-indexer-presignedurl-v1', 
      presignedUrlPayload
    );

    // Parse the body which is a string
    const responseBody = JSON.parse(presignedUrlResponse.data.body);
    
    // Extract the presigned URL
    const presignedUrl = responseBody.presigned_url;

    // Create a temporary anchor element to trigger download
    if (presignedUrl) {
      const link = document.createElement('a');
      link.href = presignedUrl;
      link.download = filename; // Set the filename for download
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
    } else {
      throw new Error('No presigned URL found');
    }
  } catch (error) {
    console.error('Error fetching presigned URL:', error);
    // Optionally show an error message to the user
    alert('Failed to download document');
  }
};

  const handleMetadataClick = (metadata) => {
    setSelectedMetadata(metadata);
    setIsModalOpen(true);
  };

// const handlePageChange = (direction) => {
//   setCurrentPage((prev) => {
//     let newPage = direction === "next" ? prev + 3 : Math.max(1, prev - 3);
//     fetchDocuments(false, newPage)
//     return newPage; // Ensures it never goes below 1
//   });
// };

// const handleRefresh = () => {
//   setCurrentPage(1)
//   fetchDocuments(true); // Pass true to ensure the refresh logic applies
// };
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
    </div>
  );
};

export default DashboardTable;



