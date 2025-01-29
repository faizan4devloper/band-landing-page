import React, { useState, useEffect } from "react";
import axios from "axios";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faRotate } from "@fortawesome/free-solid-svg-icons";
import { AnimatePresence } from "framer-motion";
import Modal from "react-modal";

import styles from "./DashboardTable.module.css";
import DocumentRow from "./DocumentRow";
import DocumentModal from "./DocumentModal";
import Pagination from "./Pagination";
import TableLoader from "./TableLoader";
import EmptyState from "./EmptyState";

// Set app element for accessibility
Modal.setAppElement("#root");

const DashboardTable = ({ userEmail }) => {
  const [documents, setDocuments] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [currentPage, setCurrentPage] = useState(0);
  const [pageSize] = useState(2);
  const [nextStartKey, setNextStartKey] = useState(null);
  const [selectedDocument, setSelectedDocument] = useState(null);
  const [isDocumentPreviewOpen, setIsDocumentPreviewOpen] = useState(false);

  const API_ENDPOINT = "https://dummy"; // Replace with actual API

  // Fetch Documents
  const fetchDocuments = async (isRefresh = false) => {
    setLoading(true);
    setError(null);

    try {
      const payload = {
        queryStringParameters: isRefresh
          ? { refresh: true, page_size: pageSize }
          : { start_page: currentPage, page_size: pageSize, ...(nextStartKey && { next_start_key: nextStartKey }) },
      };

      const response = await axios.post(API_ENDPOINT, payload, {
        headers: { "Content-Type": "application/json" },
      });

      const parsedBody = parseResponse(response.data);
      if (!parsedBody || !parsedBody.data) throw new Error("No data in response");

      const allTransactions = parsedBody.data.flatMap((page) =>
        page.transactions.map((transaction) => ({
          Filename: transaction.Filename?.S || "Unknown",
          Category: transaction.Category?.S || "Unknown",
          DateTime: transaction.DateTime?.S || new Date().toISOString(),
          TransactionID: transaction.TransactionID?.S || "Unknown",
          Metadata: Object.fromEntries(
            Object.entries(transaction.Metadata?.M || {}).map(([key, value]) => [key, value.S || "N/A"])
          ),
        }))
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
      const responseData = typeof response === "string" ? JSON.parse(response) : response;
      return typeof responseData.body === "string" ? JSON.parse(responseData.body) : responseData.body;
    } catch {
      setError("Failed to parse response");
      return null;
    }
  };

  // Open document preview modal
  const handleViewDocument = (doc) => {
    setSelectedDocument(doc);
    setIsDocumentPreviewOpen(true);
  };

  return (
    <div className={styles.documentTableContainer}>
      <div className={styles.tableHeader}>
        <h3>Dashboard</h3>
        <button onClick={() => fetchDocuments(true)} className={styles.refreshButton}>
          <FontAwesomeIcon icon={faRotate} /> Refresh
        </button>
      </div>

      <div className={styles.tableWrapper}>
        <table className={styles.documentTable}>
          <thead>
            <tr>
              <th>Filename</th>
              <th>Category</th>
              <th>Metadata</th>
              <th>Date</th>
              <th>Actions</th>
            </tr>
          </thead>
          <tbody>
            <AnimatePresence>
              {loading ? <TableLoader /> : documents.length === 0 ? <EmptyState /> : documents.map((doc) => (
                <DocumentRow key={doc.TransactionID} doc={doc} handleViewDocument={handleViewDocument} />
              ))}
            </AnimatePresence>
          </tbody>
        </table>
      </div>

      <Pagination currentPage={currentPage} setCurrentPage={setCurrentPage} hasNext={!!nextStartKey} />

      <DocumentModal
        isOpen={isDocumentPreviewOpen}
        onClose={() => setIsDocumentPreviewOpen(false)}
        document={selectedDocument}
      />
    </div>
  );
};

export default DashboardTable;







import React from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faEye, faDownload } from "@fortawesome/free-solid-svg-icons";
import { motion } from "framer-motion";
import styles from "./DocumentRow.module.css";

const DocumentRow = ({ doc, handleViewDocument }) => {
  const filename = doc.Filename ? doc.Filename.split("/").pop() : "Unknown";

  return (
    <motion.tr className={styles.documentRow} initial={{ opacity: 0, y: 20 }} animate={{ opacity: 1, y: 0 }} transition={{ duration: 0.3 }}>
      <td>{filename}</td>
      <td>{doc.Category}</td>
      <td>
        <button className={styles.seeMoreButton}>See More</button>
      </td>
      <td>{new Date(doc.DateTime).toLocaleString()}</td>
      <td>
        <div className={styles.actionButtons}>
          <button className={styles.viewButton} onClick={() => handleViewDocument(doc)}>
            <FontAwesomeIcon icon={faEye} />
          </button>
          <button className={styles.downloadButton}>
            <FontAwesomeIcon icon={faDownload} />
          </button>
        </div>
      </td>
    </motion.tr>
  );
};

export default DocumentRow;








import React from "react";
import Modal from "react-modal";
import styles from "./DocumentModal.module.css";

const DocumentModal = ({ isOpen, onClose, document }) => {
  if (!document) return null;

  return (
    <Modal isOpen={isOpen} onRequestClose={onClose} className={styles.modalContainer} overlayClassName={styles.overlay}>
      <div className={styles.modalContent}>
        <iframe src={document.url} className={styles.previewIframe} title={document.filename} />
        <button onClick={onClose} className={styles.closeModalButton}>
          Close Preview
        </button>
      </div>
    </Modal>
  );
};

export default DocumentModal;






const Pagination = ({ currentPage, setCurrentPage, hasNext }) => (
  <div className="pagination">
    <button onClick={() => setCurrentPage((prev) => Math.max(0, prev - 1))} disabled={currentPage === 0}>Previous</button>
    <span>Page {currentPage + 1}</span>
    <button onClick={() => setCurrentPage((prev) => prev + 1)} disabled={!hasNext}>Next</button>
  </div>
);

export default Pagination;
