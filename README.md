handleReload reload icons spinning infinte i dont want that i want the icon spin or load after i click and clickable untill data is displaying and then show complete status

import React, { useState } from 'react';
import Modal from 'react-modal';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faRotateRight, faSyncAlt } from '@fortawesome/free-solid-svg-icons';


const MainContent = ({ message, rows, handleReload }) => {
  const [filter, setFilter] = useState("All");
  const [isModalOpen, setIsModalOpen] = useState(false);
  const [modalContent, setModalContent] = useState(null);
  const [loadingRecNums, setLoadingRecNums] = useState([]); // Track reloading state
  const [statuses, setStatuses] = useState({}); // Store the status of each row

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

  const handleReloadClick = async (recNum) => {
    // Set status to "Reloading" immediately upon clicking
    setStatuses((prevStatuses) => ({
      ...prevStatuses,
      [recNum]: "Reloading",
    }));

    setLoadingRecNums((prev) => [...prev, recNum]); // Add recNum to loading state
    
    await handleReload(recNum);  // Simulate the reload action

    // Once the data is available, update the status to "Completed"
    const row = rows.find(row => row.recNum === recNum);
    if (row.policyid && row.type && row.summary) {
      setStatuses((prevStatuses) => ({
        ...prevStatuses,
        [recNum]: "Completed", // Update the status to "Completed" once data is loaded
      }));
    }

    setLoadingRecNums((prev) => prev.filter((id) => id !== recNum)); // Remove recNum from loading state once done
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
                <td>
                  {row.policyid ? (
                    row.policyid
                  ) : (
                    <span className={styles.loader}>Pending</span>
                  )}
                </td>
                <td>
                  {row.type ? (
                    row.type
                  ) : (
                    <span className={styles.loader}>Pending</span>
                  )}
                </td>
                <td>
                  {row.summary ? (
                    row.summary
                  ) : (
                    <span className={styles.loader}>Pending</span>
                  )}
                </td>
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
                  {statuses[row.recNum] === "Reloading" ? (
                    // Show reload icon while reloading
                    <FontAwesomeIcon
                      icon={faSyncAlt}
                      spin
                      className={styles.reloadIcon}
                    />
                  ) : statuses[row.recNum] === "Completed" ? (
                    // Show "Completed" when data is loaded
                    "Completed"
                  ) : (
                    <span>
                      Pending
                      <button
                        className={styles.reloadButton}
                        onClick={() => handleReloadClick(row.recNum)}
                        disabled={loadingRecNums.includes(row.recNum)}
                      >
                        <FontAwesomeIcon
                          icon={faSyncAlt}
                          className={styles.reloadIcon}
                        />
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



