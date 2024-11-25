import React, { useState, useEffect } from "react";
import axios from "axios";
import Sidebar from "./Sidebar";
import MainContent from "./MainContent";
import DataTable from "./DataTable";
import styles from "./ProductSheetsPage.module.css";

const ProductSheetsPage = () => {
  const [file, setFile] = useState(null);
  const [message, setMessage] = useState("");
  const [uploading, setUploading] = useState(false);
  const [rows, setRows] = useState(() => {
    // Initialize rows from localStorage, or set to empty array if not available
    const savedRows = localStorage.getItem("rows");
    return savedRows ? JSON.parse(savedRows) : [];
  });
  const [isUploaded, setIsUploaded] = useState(false);
  const [showMainContent, setShowMainContent] = useState(false);

  // Save rows to localStorage whenever they change
  useEffect(() => {
    localStorage.setItem("rows", JSON.stringify(rows));
  }, [rows]);

  const handleFileChange = (e) => {
    const selectedFile = e.target.files[0];
    if (selectedFile) {
      setFile(selectedFile);
      setMessage("");
    }
  };

  const handleUpload = async () => {
    if (!file) {
      setMessage("Please select a file to upload.");
      return;
    }
    setUploading(true);
    try {
      const sanitizedFileName = file.name.replace(/\s+/g, "_");

      const response = await axios.post("dummy", {
        payload: { filename: sanitizedFileName },
      });

      console.log("API Response 1:", response.data);

      const { presignedUrl, key, recNum } = response.data;

      await axios.put(presignedUrl, file, {
        headers: { "Content-Type": file.type },
      });

      const sqsPayload = {
        claimid: recNum,
        s3filename: key,
        actionn: "transform",
        tasktype: "SEND_TO_PS_QUEUE",
      };

      const sqsResponse = await axios.post("dummy", sqsPayload, {
        headers: { "Content-Type": "application/json" },
      });

      console.log("SQS API Response:", sqsResponse.data);

      setRows((prevRows) => [
        ...prevRows,
        {
          recNum,
          policy_id: "",
          prod_sheet_type: "",
          summary: "",
          previewLink: presignedUrl,
          status: "Pending",
        },
      ]);

      setIsUploaded(true);
    } catch (error) {
      setMessage("Upload failed: " + error.message);
    } finally {
      setUploading(false);
    }
  };

  const handleReload = async (recNum) => {
    try {
      const payload = {
        tasktype: "FETCH_SINGLE_CLAIM",
        claimid: recNum,
      };

      const response = await axios.post("dummy", payload, {
        headers: { "Content-Type": "application/json" },
      });

      console.log("API Response:", response);

      if (Array.isArray(response.data) && response.data.length > 0) {
        const singleClaimData = response.data[0];
        console.log("Single Claim Data:", singleClaimData);

        const {
          policy_id = "N/A",
          file_name = "N/A",
          status = "Unknown",
          summary = "No summary available",
          prod_sheet_type = "Unknown",
          rec_number = recNum,
        } = singleClaimData;

        setRows((prevRows) =>
          prevRows.map((row) =>
            row.recNum === recNum
              ? {
                  ...row,
                  policy_id,
                  prod_sheet_type,
                  summary,
                  status,
                  previewLink: row.previewLink,
                }
              : row
          )
        );
      } else {
        console.error("Unexpected response format:", response.data);
        setMessage("Failed to fetch claim details.");
      }
    } catch (error) {
      console.error("Error fetching claim data:", error.message);
      setMessage("Failed to fetch data for RecNum: " + recNum);
    }
  };

  return (
    <div className={styles.container}>
      {!showMainContent ? (
        <>
          <div className={styles.header}>
            <button
              className={styles.newClaimButton}
              onClick={() => setShowMainContent(true)}
            >
              New Claim Processing
            </button>
          </div>
          <DataTable rows={rows} handleReload={handleReload} />
        </>
      ) : (
        <>
          <Sidebar
            onFileChange={handleFileChange}
            onUpload={handleUpload}
            uploading={uploading}
          />
          {isUploaded ? (
            <MainContent
              message={message}
              rows={rows}
              handleReload={handleReload}
            />
          ) : (
            <p className={styles.infoMessage}>
              Please upload a document to view the data.
            </p>
          )}
        </>
      )}
    </div>
  );
};

export default ProductSheetsPage;




import React, { useState, useCallback } from 'react';
import Modal from 'react-modal';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faRotateRight, faSpinner } from '@fortawesome/free-solid-svg-icons';

const MainContent = ({ message, rows, handleReload }) => {
  const [filter, setFilter] = useState("All");
  const [isModalOpen, setIsModalOpen] = useState(false);
  const [modalContent, setModalContent] = useState(null);
  const [loadingRows, setLoadingRows] = useState([]);

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

  // useCallback to avoid unnecessary re-renders
  const handleRowReload = useCallback(
    async (recNum) => {
      setLoadingRows((prev) => [...prev, recNum]); // Add the row to the loading state
      await handleReload(recNum);
      setLoadingRows((prev) => prev.filter((id) => id !== recNum)); // Remove the row from the loading state
    },
    [handleReload] // Ensure the function is only re-created if handleReload changes
  );

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
                        onClick={() => handleRowReload(row.recNum)}
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
