import React, { useState } from "react";
import Modal from "react-modal";
import styles from "./MainContent.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faRotateRight } from "@fortawesome/free-solid-svg-icons";

const MainContent = ({ message, rows, handleReload }) => {
  const [filter, setFilter] = useState("All");
  const [isModalOpen, setIsModalOpen] = useState(false);
  const [modalContent, setModalContent] = useState(null);
  const [loadingRows, setLoadingRows] = useState([]); // Track rows being reloaded

  // Filter rows based on the selected filter
  const filteredRows = rows.filter((row) =>
    filter === "All" ? true : row.status.includes(filter)
  );

  const handleFilterChange = (e) => {
    setFilter(e.target.value);
  };

  const openModal = (previewLink) => {
    setModalContent(previewLink);
    setIsModalOpen(true);
  };

  const closeModal = () => {
    setIsModalOpen(false);
    setModalContent(null);
  };

  const handleRowReload = async (recNum) => {
    // Add the row to loading state
    setLoadingRows((prev) => [...prev, recNum]);

    // Simulate data fetching (replace with actual reload logic)
    await new Promise((resolve) => setTimeout(resolve, 2000));

    // Update the row's status to "Completed"
    const updatedRows = rows.map((row) =>
      row.recNum === recNum ? { ...row, status: "Completed" } : row
    );

    // Remove the row from loading state
    setLoadingRows((prev) => prev.filter((id) => id !== recNum));
  };

  return (
    <div className={styles.mainContent}>
      {message && <p className={styles.message}>{message}</p>}

      {/* Filter Dropdown */}
      <div className={styles.filterContainer}>
        <label htmlFor="statusFilter" className={styles.filterLabel}>
          Filter by Status:
        </label>
        <select
          id="statusFilter"
          className={styles.filterDropdown}
          value={filter}
          onChange={handleFilterChange}
        >
          <option value="All">All</option>
          <option value="Pending">Pending</option>
          <option value="Completed">Completed</option>
        </select>
      </div>

      {/* Table */}
      <table className={styles.table}>
        <thead>
          <tr>
            <th>RecNum</th>
            <th>Policy ID</th>
            <th>Product Sheet Type</th>
            <th>Summary</th>
            <th>Preview Link</th>
            <th>Status</th>
          </tr>
        </thead>
        <tbody>
          {filteredRows.length > 0 ? (
            filteredRows.map((row, index) => (
              <tr key={index}>
                <td>{row.recNum}</td>
                <td>{row.policyid || <span className={styles.loader}>Pending</span>}</td>
                <td>{row.type || <span className={styles.loader}>Pending</span>}</td>
                <td>{row.summary || <span className={styles.loader}>Pending</span>}</td>
                <td>
                  {row.previewLink ? (
                    <button
                      className={styles.previewButton}
                      onClick={() => openModal(row.previewLink)}
                    >
                      Preview
                    </button>
                  ) : (
                    "Pending"
                  )}
                </td>
                <td>
                  {row.status === "Pending" ? (
                    <span>
                      Pending
                      <button
                        className={`${styles.reloadButton} ${
                          loadingRows.includes(row.recNum) ? "spinning" : ""
                        }`}
                        onClick={() => handleRowReload(row.recNum)}
                        disabled={loadingRows.includes(row.recNum)}
                      >
                        <FontAwesomeIcon
                          icon={faRotateRight}
                          className={loadingRows.includes(row.recNum) ? "spinning" : ""}
                        />
                      </button>
                    </span>
                  ) : (
                    row.status
                  )}
                </td>
              </tr>
            ))
          ) : (
            <tr>
              <td colSpan="6">No data available</td>
            </tr>
          )}
        </tbody>
      </table>

      {/* Modal */}
      <Modal
        isOpen={isModalOpen}
        onRequestClose={closeModal}
        className={styles.modal}
        overlayClassName={styles.modalOverlay}
        ariaHideApp={false}
      >
        <div className={styles.modalContent}>
          <button className={styles.closeButton} onClick={closeModal}>
            Close
          </button>
          {modalContent ? (
            <iframe
              src={modalContent}
              title="Preview"
              className={styles.modalIframe}
            ></iframe>
          ) : (
            <p>No preview available</p>
          )}
        </div>
      </Modal>
    </div>
  );
};

export default MainContent;



@keyframes spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

.reloadButton .spinning {
  animation: spin 1s linear infinite;
}
