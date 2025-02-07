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
