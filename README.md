import React, { useState } from 'react';
import axios from 'axios';
import Modal from 'react-modal';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faRotateRight, faSpinner } from '@fortawesome/free-solid-svg-icons';

const MainContent = ({ message, rows, setRows }) => {
  const [filter, setFilter] = useState("All");
  const [isModalOpen, setIsModalOpen] = useState(false);
  const [modalContent, setModalContent] = useState(null);
  const [loadingRows, setLoadingRows] = useState([]);

  const handleReload = async (recNum) => {
    setLoadingRows((prev) => [...prev, recNum]); // Add the row to the loading state
    try {
      const payload = {
        tasktype: "FETCH_SINGLE_CLAIM",
        claimid: recNum,
      };

      const response = await axios.post(`dummy`, payload, {
        headers: { "Content-Type": "application/json" },
      });

      const singleclaimdata = response.data.singleclaimdata[0] || {};
      console.log("Reload Data:", response.data);
      // console.log("single Data:", singleclaimdata.data);
      console.log("policy Data:", response.data.singleclaimdata[0].policy_id);

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
      console.log("Update rows:", rows)
      
    } catch (error) {
      console.error("Failed to fetch data for RecNum:", recNum, error);
    } finally {
      setLoadingRows((prev) => prev.filter((id) => id !== recNum)); // Remove the row from the loading state
    }
  };

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
          onChange={handleFilterChange}
        >
          <option value="All">All</option>
          <option value="Pending">Pending</option>
          <option value="Completed">Completed</option>
        </select>
      </div>

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
                <td>{row.recNum || <span className={styles.loader}>Pending</span>}</td>
                <td>{row.policy_id || <span className={styles.loader}>Pending</span>}</td>
                <td>{row.prod_sheet_type || <span className={styles.loader}>Pending</span>}</td>
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
                  {row.policy_id && row.prod_sheet_type && row.summary ? (
                    <span>Completed</span>
                  ) : (
                    <span>
                      <button
                        className={styles.reloadButton}
                        onClick={() => handleReload(row.recNum)}
                        disabled={loadingRows.includes(row.recNum)}
                      >
                        {loadingRows.includes(row.recNum) ? (
                          <FontAwesomeIcon
                            icon={faSpinner}
                            spin
                            className={styles.spinnerIcon}
                          />
                        ) : (
                          <FontAwesomeIcon icon={faRotateRight} />
                        )}
                      </button>
                    </span>
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





/* Main Content Styles */
.mainContent {
  margin-left: 20px;
  flex-grow: 1;
  padding: 20px;
}

/* Message Styling */
.message {
  padding: 10px;
  background-color: #f9f9f9;
  border-left: 5px solid #007bff;
  margin-bottom: 20px;
  font-size: 16px;
  color: #333;
}

/* Table Styling */
.table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
  font-size: 14px;
}

.table th, .table td {
  padding: 12px;
  border: 1px solid #ddd;
  text-align: center;
}

.table th {
  background: #0f5fdc;
  color: white;
  font-weight: bold;
}

.table tr:nth-child(even) {
  background-color: #f9f9f9;
}

.table tr:hover {
  background-color: #f1f1f1;
}

/* Loader */
@keyframes spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

/* Buttons */
.reloadButton {
      background: none;
    border: none;
    color: #007bff;
    cursor: pointer;
    margin-left: 2px;
    padding-top: 4px;
    transition: .3s ease;
    font-size: 14px;
}

.reloadButton:hover {
  color: darkblue;
}

.previewButton {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 6px 10px;
  cursor: pointer;
  border-radius: 4px;
}

.previewButton:hover {
  background-color: #0056b3;
}

/* Modal Styling */
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
  overflow: hidden;
}

.modalContent {
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
  border: none;
  padding: 6px 10px;
  cursor: pointer;
  border-radius: 4px;
  float: right;
}

.closeButton:hover {
  background: darkred;
}

.filterContainer{
  margin-bottom: 20px;
  display: flex;
  align-items: center;
  gap: 10px;
}

.filterLabel{
  font-size: 16px;
  font-weight: 500;
}

.filterDropdown{
     padding: 8px 12px;
    border: 1.5px solid #ccc;
    border-radius: 4px;
    cursor: pointer;
    flex-wrap: wrap;

}

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
