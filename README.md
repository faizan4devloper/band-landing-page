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
  const [refreshing, setRefreshing] = useState(false);

  const handleReload = async (recNum) => {
    setLoadingRows((prev) => [...prev, recNum]);
    try {
      const payload = {
        tasktype: "FETCH_SINGLE_ACT_CLAIM",
        claimid: recNum,
      };

      const response = await axios.post(`dummy`, payload, {
        headers: { "Content-Type": "application/json" },
      });
      
      console.log(response.data)

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
      setLoadingRows((prev) => prev.filter((id) => id !== recNum));
    }
  };

  const handleRefresh = async () => {
    setRefreshing(true);
    try {
      // Simulate an API call to refresh all data
      const payload = {
        tasktype: "FETCH_ALL_CLAIMS",
      };

      const response = await axios.post(`dummy`, payload, {
        headers: { "Content-Type": "application/json" },
      });

      // Assuming the refreshed data is returned as an array
      setRows(response.data.claims || []);
    } catch (error) {
      console.error("Failed to refresh data:", error);
    } finally {
      setRefreshing(false);
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
      <div className={styles.previewSection}>
        <h3>Preview</h3>
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

      <div className={styles.extractContentSection}>
        <div className={styles.refreshContainer}>
          <h3>Extract Content</h3>
          <button
            className={styles.refreshButton}
            onClick={handleRefresh}
            disabled={refreshing}
          >
            {refreshing ? (
              <FontAwesomeIcon
                icon={faSpinner}
                spin
                className={styles.spinnerIcon}
              />
            ) : (
              <FontAwesomeIcon icon={faRotateRight} />
            )}
          </button>
        </div>

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
      </div>
    </div>
  );
};

export default MainContent;
