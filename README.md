import React from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import {
  faChevronLeft,
  faChevronRight,
} from "@fortawesome/free-solid-svg-icons";
import styles from "./DashboardTable.module.css";

const TableData = ({
  documents,
  loading,
  currentPage,
  nextStartKey,
  handlePageChange,
  handleMetadataClick
}) => {

  const renderDocumentRow = (doc) => (
    <tr key={doc.TransactionID}>
      <td>{doc.Filename.split("/").pop()}</td>
      <td>{doc.Category}</td>
      <td>
        <button
          onClick={() => handleMetadataClick(doc.Metadata)}
        >
          See More
        </button>
      </td>
      <td>{new Date(doc.DateTime).toLocaleString()}</td>
    </tr>
  );

  const renderTableContent = () => {
    if (loading) {
      return (
        <tr>
          <td colSpan="5" className={styles.loadingRow}>
            Loading...
          </td>
        </tr>
      );
    }

    if (documents.length === 0) {
      return (
        <tr>
          <td colSpan="5" className={styles.emptyRow}>
            No documents found
          </td>
        </tr>
      );
    }

    return documents.map(renderDocumentRow);
  };

  const renderPagination = () => {
    const totalPages = Math.ceil(documents.length / 10); // Example logic
    const pageNumbers = [];
    for (let i = 1; i <= totalPages; i++) {
      pageNumbers.push(i);
    }

    return (
      <div className={styles.pagination}>
        <button
          className={styles.paginationButton}
          onClick={() => handlePageChange("prev")}
          disabled={currentPage === 0}
        >
          <FontAwesomeIcon icon={faChevronLeft} />
          Prev
        </button>

        <div className={styles.pageNumbers}>
          {currentPage > 2 && (
            <>
              <span>...</span>
              <button onClick={() => handlePageChange(0)}>1</button>
            </>
          )}
          {pageNumbers.slice(currentPage - 1, currentPage + 2).map((num) => (
            <button
              key={num}
              className={num === currentPage + 1 ? styles.activePage : ""}
              onClick={() => handlePageChange(num - 1)}
            >
              {num}
            </button>
          ))}
          {currentPage < totalPages - 3 && (
            <>
              <span>...</span>
              <button onClick={() => handlePageChange(totalPages - 1)}>
                {totalPages}
              </button>
            </>
          )}
        </div>

        <button
          className={styles.paginationButton}
          onClick={() => handlePageChange("next")}
          disabled={currentPage === totalPages - 1}
        >
          Next
          <FontAwesomeIcon icon={faChevronRight} />
        </button>
      </div>
    );
  };

  return (
    <div>
      <table>
        <thead>
          <tr>
            <th>Filename</th>
            <th>Category</th>
            <th>Metadata</th>
            <th>Date</th>
          </tr>
        </thead>
        <tbody>
          {renderTableContent()}
        </tbody>
      </table>

      {renderPagination()}
    </div>
  );
};

export default TableData;


.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 10px;
  margin-top: 20px;
}

.paginationButton {
  padding: 8px 12px;
  background-color: #4a90e2;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.pageNumbers button {
  background-color: #f4f4f4;
  padding: 6px 12px;
  border: 1px solid #ddd;
  cursor: pointer;
}

.pageNumbers .activePage {
  background-color: #4a90e2;
  color: white;
}

.paginationButton:disabled {
  background-color: #ddd;
  cursor: not-allowed;
}
