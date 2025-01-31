import React from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import {
  faSync,
  faEye,
  faDownload,
  faChevronLeft,
  faChevronRight,
} from "@fortawesome/free-solid-svg-icons";
import styles from "./DashboardTable.module.css";

const TableData = ({ documents, loading, currentPage, handlePageChange }) => {
  return (
    <div className={styles.tableWrapper}>
      <table className={styles.documentTable}>
        <thead>
          <tr>
            <th>Filename</th>
            <th>Category</th>
            <th>Metadata</th>
            <th>Date</th>
          </tr>
        </thead>
        <tbody>
          {loading ? (
            <tr>
              <td colSpan="4" className={styles.loadingRow}>
                <div className={styles.loadingContent}>
                  <FontAwesomeIcon icon={faSync} spin className={styles.loadingIcon} />
                  <p>Loading documents...</p>
                </div>
              </td>
            </tr>
          ) : documents.length === 0 ? (
            <tr>
              <td colSpan="4" className={styles.emptyRow}>
                <p>No documents found</p>
              </td>
            </tr>
          ) : (
            documents.map((doc) => (
              <tr key={doc.TransactionID} className={styles.documentRow}>
                <td>{doc.Filename.split("/").pop()}</td>
                <td>{doc.Category}</td>
                <td>{JSON.stringify(doc.Metadata)}</td>
                <td>{new Date(doc.DateTime).toLocaleString()}</td>
              </tr>
            ))
          )}
        </tbody>
      </table>

      {/* Pagination */}
      <div className={styles.pagination}>
        <button
          className={`${styles.paginationButton} ${currentPage === 1 ? styles.disabled : ""}`}
          onClick={() => handlePageChange("prev")}
          disabled={currentPage === 1}
        >
          <FontAwesomeIcon icon={faChevronLeft} className={styles.paginationIcon} />
          Prev
        </button>

        <div className={styles.paginationInfo}>Page {currentPage}</div>

        <button
          className={`${styles.paginationButton} ${documents.length === 0 ? styles.disabled : ""}`}
          onClick={() => handlePageChange("next")}
          disabled={documents.length === 0}
        >
          Next
          <FontAwesomeIcon icon={faChevronRight} className={styles.paginationIcon} />
        </button>
      </div>
    </div>
  );
};

export default TableData;
