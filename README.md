import React, { useState } from "react";
import axios from "axios";
import Modal from "react-modal";
import styles from "./MainContent.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faRotateRight, faSpinner, faEye } from "@fortawesome/free-solid-svg-icons";

const MainContent = ({ message, rows, setRows }) => {
  const [filter, setFilter] = useState("All");
  const [isModalOpen, setIsModalOpen] = useState(false);
  const [modalContent, setModalContent] = useState(null);
  const [loadingRows, setLoadingRows] = useState([]);

  const handleReload = async (recNum) => {
    setLoadingRows((prev) => [...prev, recNum]); // Add row to loading state
    try {
      const payload = {
        tasktype: "FETCH_SINGLE_CLAIM",
        claimid: recNum,
      };

      const response = await axios.post("dummy", payload, {
        headers: { "Content-Type": "application/json" },
      });

      const singleclaimdata = response.data.singleclaimdata[0] || {};

      setRows((prevRows) =>
        prevRows.map((row) =>
          row.recNum === recNum
            ? {
                ...row,
                policy_id: singleclaimdata.policy_id,
                prod_sheet_type: singleclaimdata.prod_sheet_type,
                summary: singleclaimdata.summary,
                status: singleclaimdata.status,
                previewLink: singleclaimdata.previewLink,
              }
            : row
        )
      );
    } catch (error) {
      console.error("Failed to fetch data for RecNum:", recNum, error);
    } finally {
      setLoadingRows((prev) => prev.filter((id) => id !== recNum)); // Remove row from loading state
    }
  };

  const filteredRows = rows.filter((row) =>
    filter === "All" ? true : row.status.includes(filter)
  );

  const openModal = (previewLink) => {
    setModalContent(previewLink);
    setIsModalOpen(true);
  };

  const closeModal = () => {
    setIsModalOpen(false);
    setModalContent(null);
  };

  return (
    <div className={styles.mainContent}>
      {message && <p className={styles.message}>{message}</p>}

      <div className={styles.filterContainer}>
        <label htmlFor="statusFilter" className={styles.filterLabel}>
          Filter by Status:
        </label>
        <select
          id="statusFilter"
          className={styles.filterDropdown}
          value={filter}
          onChange={(e) => setFilter(e.target.value)}
        >
          <option value="All">All</option>
          <option value="Pending">Pending</option>
          <option value="Completed">Completed</option>
        </select>
      </div>

      <table className={styles.table}>
        <thead>
          <tr>
            <th>Policy ID</th>
            <th>Product Sheet Type</th>
            <th>Summary</th>
            <th>Status</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          {filteredRows.length > 0 ? (
            filteredRows.map((row, index) => (
              <tr key={index}>
                <td>{row.policy_id || <span className={styles.loader}>Loading...</span>}</td>
                <td>{row.prod_sheet_type || <span className={styles.loader}>Loading...</span>}</td>
                <td>{row.summary || <span className={styles.loader}>Loading...</span>}</td>
                <td>
                  {row.policy_id && row.prod_sheet_type && row.summary ? (
                    <span className={styles.completedStatus}>Completed</span>
                  ) : (
                    <button
                      className={styles.reloadButton}
                      onClick={() => handleReload(row.recNum)}
                      disabled={loadingRows.includes(row.recNum)}
                    >
                      {loadingRows.includes(row.recNum) ? (
                        <FontAwesomeIcon icon={faSpinner} spin className={styles.spinnerIcon} />
                      ) : (
                        <FontAwesomeIcon icon={faRotateRight} />
                      )}
                    </button>
                  )}
                </td>
                <td>
                  {row.previewLink && (
                    <button className={styles.previewButton} onClick={() => openModal(row.previewLink)}>
                      <FontAwesomeIcon icon={faEye} /> Preview
                    </button>
                  )}
                </td>
              </tr>
            ))
          ) : (
            <tr>
              <td colSpan="5">No data available</td>
            </tr>
          )}
        </tbody>
      </table>

      <Modal
        isOpen={isModalOpen}
        onRequestClose={closeModal}
        className={styles.modal}
        overlayClassName={styles.modalOverlay}
        ariaHideApp={false}
      >
        <div className={styles.modalContent}>
          <button className={styles.closeButton} onClick={closeModal}>Close</button>
          {modalContent ? (
            <iframe src={modalContent} title="Preview" className={styles.modalIframe}></iframe>
          ) : (
            <p>No preview available</p>
          )}
        </div>
      </Modal>
    </div>
  );
};

export default MainContent;




.mainContent {
  padding: 20px;
}

.message {
  padding: 10px;
  background-color: #f9f9f9;
  border-left: 5px solid #007bff;
  font-size: 16px;
  color: #333;
}

.table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
  font-size: 14px;
  color: #333;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.table th,
.table td {
  padding: 12px;
  border: 1px solid #ddd;
}

.table th {
  background: #4a90e2;
  color: white;
  text-transform: uppercase;
  font-size: 13px;
}

.table tr:nth-child(even) {
  background-color: #f8f9fa;
}

.table tr:hover {
  background-color: #e8f0fe;
}

.completedStatus {
  color: #28a745;
  font-weight: bold;
}

.loader {
  font-style: italic;
  color: #aaa;
}

.reloadButton {
  background: none;
  border: none;
  color: #357ae8;
  cursor: pointer;
  font-size: 14px;
}

.reloadButton:hover {
  color: #1d5dbf;
}

.previewButton {
  background-color: #4a90e2;
  color: white;
  border: none;
  padding: 8px 12px;
  cursor: pointer;
  border-radius: 4px;
}

.previewButton:hover {
  background-color: #357ae8;
}

.modalOverlay {
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  background: white;
  width: 80%;
  max-width: 600px;
  border-radius: 8px;
  padding: 20px;
}

.modalIframe {
  width: 100%;
  height: 400px;
  border: none;
}

.closeButton {
  background: red;
  color: white;
  padding: 6px 10px;
  cursor: pointer;
}
