API: 
https:same api for all thing
Paginations:
 
refresh:
{
    "action_type": "pagination",
    "refresh": true,
    "start_page": 1,
    "page_size": 20
}
 
1st three pages: you can use true or false for refresh here
 
{
    "action_type": "pagination",
    "refresh": true,
    "start_page": 1,
    "page_size": 20
}
 
page 4,5,6 and so on:
 
{
    "action_type": "pagination",
    "refresh": false,
    "start_page": 4,
    "page_size": 20
}
 
Tile 2: Previous day
 
{
    "action_type": "transaction_counts",
    "time_period": "previous_day"
}
 
Tile 3: Day before yesterday
{
    "action_type": "transaction_counts",
    "time_period": "day_before_yesterday"
}
 
Tile 4: last month
 
{
    "action_type": "transaction_counts",
    "time_period": "last_month"
}
 
![image](https://github.com/user-attachments/assets/dddf61ca-9c4a-468e-818c-9ca486954db2)

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
  const [pageSize] = useState(2);
  const [nextStartKey, setNextStartKey] = useState(null);
  const [selectedMetadata, setSelectedMetadata] = useState(null);
  const [isModalOpen, setIsModalOpen] = useState(false);
  const [selectedDocument, setSelectedDocument] = useState(null);
  const [isDocumentPreviewOpen, setIsDocumentPreviewOpen] = useState(false);

  const API_ENDPOINT = "dummy"; // Replace with the actual API endpoint

  const fetchDocuments = async (isRefresh = false) => {
    setLoading(true);
    setError(null);
    try {
      const payload = {
        // queryStringParameters: isRefresh
        //   ? { refresh: true, page_size: pageSize }
        //   : {
        //       start_page: currentPage,
        //       page_size: pageSize,
        //       ...(nextStartKey && { next_start_key: nextStartKey }),
        //     },
        
    action_type: "pagination",
    refresh: true,
    start_page: 1,
    page_size: 20

      };

      const response = await axios.post(API_ENDPOINT, payload, {
        headers: { "Content-Type": "application/json" },
      });

console.log('All Data:', response)
      const parsedBody = parseResponse(response.data);

      if (!parsedBody || !parsedBody.data) {
        throw new Error("No data in response");
      }

      const allTransactions = parsedBody.data.flatMap((page) =>
        page.transactions
          .map((transaction) => {
            try {
              return {
                Filename: transaction.Filename?.S || "Unknown",
                Category: transaction.Category?.S || "Unknown",
                DateTime: transaction.DateTime?.S || new Date().toISOString(),
                TransactionID: transaction.TransactionID?.S || "Unknown",
                Metadata: Object.fromEntries(
                  Object.entries(transaction.Metadata?.M || {}).map(
                    ([key, value]) => [key, value.S || "N/A"]
                  )
                ),
              };
            } catch {
              return null;
            }
          })
          .filter(Boolean)
      );

      setDocuments(allTransactions);
      setNextStartKey(parsedBody.next_start_key || null);
    } catch (err) {
      setError(err.message || "Failed to fetch documents");
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
      'dummy', 
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
      'dummy', 
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

  const handlePageChange = (direction) => {
    if (direction === "next" && documents.length > 0) {
      setCurrentPage((prev) => prev + 1);
    } else if (direction === "prev" && currentPage > 0) {
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



import React, { useState, useEffect } from 'react';
import axios from 'axios';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { 
  faChartLine, 
  faClipboardList, 
  faChartPie, 
  faCalendarCheck 
} from '@fortawesome/free-solid-svg-icons';
import styles from './Summary.module.css';

const Summary = () => {
  const [tile1Data, setTile1Data] = useState([]);
  const [tile2Data, setTile2Data] = useState({ total: 0, successful: 0 });
  const [tile3Data, setTile3Data] = useState({ total: 0, successful: 0 });
  const [tile4Data, setTile4Data] = useState({ total: 0, successful: 0 });
  const [currentPage, setCurrentPage] = useState(1);

  // APIs
  const tile1Api = 'API_FOR_TILE_1'; // Replace with actual API endpoint
  const tile2Api = 'API_FOR_TILE_2';
  const tile3Api = 'API_FOR_TILE_3';
  const tile4Api = 'API_FOR_TILE_4';

  // Fetch data for Tile 1
  useEffect(() => {
    const fetchTile1Data = async () => {
      try {
        const responses = await Promise.all(
          Array.from({ length: 3 }, (_, i) =>
            axios.get(`${tile1Api}?page=${i + 1}`)
          )
        );
        setTile1Data(responses.map((res) => res.data));
      } catch (error) {
        console.error('Error fetching Tile 1 data:', error);
      }
    };
    fetchTile1Data();
  }, [tile1Api]);

  // Fetch data for Tile 2
  useEffect(() => {
    const fetchTile2Data = async () => {
      try {
        const response = await axios.get(tile2Api);
        setTile2Data(response.data);
      } catch (error) {
        console.error('Error fetching Tile 2 data:', error);
      }
    };
    fetchTile2Data();
  }, [tile2Api]);

  // Fetch data for Tile 3
  useEffect(() => {
    const fetchTile3Data = async () => {
      try {
        const response = await axios.get(tile3Api);
        setTile3Data(response.data);
      } catch (error) {
        console.error('Error fetching Tile 3 data:', error);
      }
    };
    fetchTile3Data();
  }, [tile3Api]);

  // Fetch data for Tile 4
  useEffect(() => {
    const fetchTile4Data = async () => {
      try {
        const response = await axios.get(tile4Api);
        setTile4Data(response.data);
      } catch (error) {
        console.error('Error fetching Tile 4 data:', error);
      }
    };
    fetchTile4Data();
  }, [tile4Api]);
  return (
<div className={styles.summaryContainer}>
      {/* Tile 1 */}
      <div className={`\${styles.tile} \${styles.tile1}`}>
        <div className={styles.tileHeader}>
          <FontAwesomeIcon icon={faChartLine} className={styles.tileIcon} />
          <h4>Pagination Data</h4>
        </div>
        <div className={styles.tileContent}>
          <ul>
            {tile1Data.flatMap((page, index) =>
              page.map((item, idx) => (
                <li key={`\${index}-\${idx}`}>
                  {item.date} - {item.time}
                </li>
              ))
            )}
          </ul>
        </div>
      </div>

      {/* Tile 2 */}
      <div className={`\${styles.tile} \${styles.tile2}`}>
        <div className={styles.tileHeader}>
          <FontAwesomeIcon icon={faClipboardList} className={styles.tileIcon} />
          <h4>Today's Transactions</h4>
        </div>
        <div className={styles.tileContent}>
          <div className={styles.statItem}>
            <span>Total</span>
            <span className={styles.statValue}>{tile2Data.total}</span>
          </div>
          <div className={styles.statItem}>
            <span>Successful</span>
            <span className={styles.statValue}>{tile2Data.successful}</span>
          </div>
        </div>
      </div>

      {/* Tile 3 */}
      <div className={`\${styles.tile} \${styles.tile3}`}>
        <div className={styles.tileHeader}>
          <FontAwesomeIcon icon={faChartPie} className={styles.tileIcon} />
          <h4>Transaction Counts</h4>
        </div>
        <div className={styles.tileContent}>
          <div className={styles.statItem}>
            <span>Total</span>
            <span className={styles.statValue}>{tile3Data.total}</span>
          </div>
          <div className={styles.statItem}>
            <span>Successful</span>
            <span className={styles.statValue}>{tile3Data.successful}</span>
          </div>
        </div>
      </div>

      {/* Tile 4 */}
      <div className={`\${styles.tile} \${styles.tile4}`}>
        <div className={styles.tileHeader}>
          <FontAwesomeIcon icon={faCalendarCheck} className={styles.tileIcon} />
          <h4>Last Month's Transactions</h4>
        </div>
        <div className={styles.tileContent}>
          <div className={styles.statItem}>
            <span>Total</span>
            <span className={styles.statValue}>{tile4Data.total}</span>
          </div>
          <div className={styles.statItem}>
            <span>Successful</span>
            <span className={styles.statValue}>{tile4Data.successful}</span>
          </div>
        </div>
      </div>
    </div>  );
};

export default Summary;
