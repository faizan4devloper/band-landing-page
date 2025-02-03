
import React, { useEffect, useState } from "react";
import axios from "axios";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faSyncAlt } from "@fortawesome/free-solid-svg-icons";
import { HashLoader } from "react-spinners";
import styles from "./AllDataTable.module.css";
import Modal from 'react-modal';

Modal.setAppElement('#root');

const AllDataTable = () => {
  const [rows, setRows] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState("");
  const [spinningRows, setSpinningRows] = useState({});
    const [selectedRow, setSelectedRow] = useState(null); // State to track selected row
  const [isModalOpen, setIsModalOpen] = useState(false)

  const fetchData = async () => {
    setLoading(true);
    setError("");
    try {
      const payload = {
        tasktype: "FETCH_ALL_ACT_CLAIMS", // Different payload
        // "tasktype" : "FETCH_ALL_ACT_CLAIMS"
      };

      const headers = {
        "Content-Type": "application/json",
      };

      const response = await axios.post("dummy", payload, { headers }); // Different API URL

      console.log("API Response:", response.data);

      const allData = Object.values(response.data.allclaimactdata || {});
      console.log("Extracted All Data:", allData);

      setRows(allData);
    } catch (err) {
      setError("Failed to fetch data. Please try again.");
      console.error("API Error:", err);
    } finally {
      setLoading(false);
    }
  };
  
   const openModal = (row) => {
    setSelectedRow(row); // Set the selected row data
    setIsModalOpen(true); // Open the modal
  };

  const closeModal = () => {
    setIsModalOpen(false); // Close the modal
    setSelectedRow(null); // Clear the selected row data
  };

  const handleReloadRow = async (uniqueKey) => {
    setSpinningRows((prev) => ({ ...prev, [uniqueKey]: true }));
    try {
      console.log(`Reloading data for uniqueKey: ${uniqueKey}`);
      await new Promise((resolve) => setTimeout(resolve, 1000));
    } catch (err) {
      console.error("Error reloading row:", err);
    } finally {
      setSpinningRows((prev) => ({ ...prev, [uniqueKey]: false }));
    }
  };

  useEffect(() => {
    fetchData();
  }, []);

  return (
    <div className={styles.tableContainer}>
      {loading ? (
        <div className={styles.spinnerContainer}>
          <HashLoader color="#0f5fdc" size={40} />
        </div>
      ) : error ? (
        <p className={styles.error}>{error}</p>
      ) : rows.length > 0 ? (
        <table className={styles.table}>
          <thead>
            <tr>
              <th>Claim ID</th>
              <th>Summary</th>
              <th width="12%">claim Type</th>
              <th>Status</th>
            </tr>
          </thead>
          <tbody>
            {rows.map((row, index) => {
              const uniqueKey = `${index}_${row.uniqueField}`; // Adjust unique key based on available data
              return (
                <tr key={uniqueKey}>
                  <td className={styles.clickable} onClick={()=> openModal(row)}>{row.claimid}</td>
                  <td>{row.briefsummary}</td>
                  <td>{row.claimtype}</td>
                  <td>{row.status}</td>
                 
                </tr>
              );
            })}
          </tbody>
        </table>
      ) : (
        <p className={styles.noData}>No data available</p>
      )}
       {/* Modal */}
      <Modal
        isOpen={isModalOpen}
        onRequestClose={closeModal}
        className={styles.modal}
        overlayClassName={styles.modalOverlay}
      >
        {selectedRow && (
          <div className={styles.modalContent}>
        <h2 className={styles.modalTitle}>{selectedRow.claimid} Details</h2>
            <p>
              <strong>Brief Summary:</strong> {selectedRow.briefsummary}
            </p>
            <p>
              <strong>Claim Type:</strong> {selectedRow.claimtype}
            </p>
            <p>
              <strong>Claim Status:</strong> {selectedRow.status}
            </p>
            
          </div>
        )}
        <button onClick={closeModal} className={styles.modalCloseButton}>
          Close
        </button>
      </Modal>
    </div>
  );
};

export default AllDataTable;


/* Container for the table */
.tableContainer {
  /*margin-top: 40px;*/
  overflow-x: auto;
  border-radius: 10px;
  /*box-shadow: 0px 4px 12px rgba(0, 0, 0, 0.15);*/
  /*background-color: #fff;*/
  padding: 5px 80px;
}

.table {
  width: 100%;
  border-collapse: collapse;
  /*margin-top: 20px;*/
  color: #333; /* Text color */
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1); /* Subtle shadow for table */
}

.table th,
.table td {
  padding: 15px; /* Increased padding for better spacing */
  border: 1px solid #ddd; /* Subtle border */
  text-align: left; /* Align text to the left for consistency */
}

.table th {
  background: linear-gradient(135deg, #4a90e2, #357ae8); /* Gradient background */
  color: #fff; /* White text for contrast */
  font-weight: 600; /* Bolder text */
  text-transform: uppercase; /* Uppercase headers for distinction */
  font-size: 16px; /* Slightly smaller font for headers */
}

.table tr:nth-child(even) {
  background-color: #f8f9fa; /* Light grey for alternate rows */
}

.table tr:hover {
  background-color: #e8f0fe; /* Subtle blue hover effect */
}

.table td {
     font-size: 16px;
    font-weight: 450;
    line-height: 1.5;
}

.reloadButton {
  background: none;
  border: none;
  color: #357ae8;
  cursor: pointer;
  margin: 0;
  padding: 6px;
  font-size: 14px;
  transition: color 0.3s ease;
}

.reloadButton:hover {
  color: #1d5dbf; /* Darker blue for hover state */
}

.previewButton {
  background-color: #4a90e2;
  color: white;
  border: none;
  padding: 8px 12px;
  cursor: pointer;
  border-radius: 4px;
  transition: background-color 0.3s ease;
  font-size: 14px; /* Consistent font size */
}

.previewButton:hover {
  background-color: #357ae8; /* Darker blue hover effect */
}

/* Empty Table Message */
.table-empty {
  text-align: center;
  color: #999;
  font-size: 16px;
  padding: 20px;
}

/* Spinner container */
.spinnerContainer {
  /*display: flex;*/
  position: absolute;
  right:50%;
  top:50%;
}

/* Error styling */
.error {
  color: red;
  font-weight: bold;
  text-align: center;
  margin-top: 20px;
}

/* No data styling */
.noData {
  text-align: center;
  color: #999;
  padding: 20px;
  font-style: italic;
  font-size: 18px;
}

/* Reload button styling */
.reloadButton {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 14px;
  transition: all 0.3s ease-in-out;
}

.reloadButton:hover {
  background-color: #0056b3;
  transform: scale(1.05);
}

/* Reload icon */
.reloadIcon {
  font-size: 16px;
}

/* Spinning animation */
.fa-spin {
  animation: spin 1s linear infinite;
}
/* Table styling (clickable Rec No.) */
.clickable {
  cursor: pointer;
  color: #007bff;
  text-decoration: underline;
}

.clickable:hover {
  color: #0056b3;
}

/* Modal Overlay */
.modalOverlay {
  background-color: rgba(0, 0, 0, 0.7);
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
}

/* Modal Content */
.modal {
  background: #fff;
  border-radius: 10px;
  padding: 20px;
  width: 500px;
  max-width: 90%;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
  animation: slideIn 0.3s ease-out;
}

.modalTitle {
  font-size: 1.5rem;
  margin-bottom: 15px;
  color: #333;
}

.modalContent {
  font-size: 1rem;
  line-height: 1.6;
  color: #555;
}

.modalCloseButton {
  background-color: #007bff;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  font-size: 1rem;
  cursor: pointer;
  margin:auto 200px;
  margin-top: 20px;
  transition: background-color 0.3s ease;
}

.modalCloseButton:hover {
  background-color: #0056b3;
}

/* Modal Animation */
@keyframes slideIn {
  from {
    transform: translateY(-50px);
    opacity: 0;
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}

@keyframes spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

.paginationContainer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 20px;
  padding: 15px;
  background-color: #f8f9fa;
  border-radius: 8px;
}

.paginationControls {
  display: flex;
  align-items: center;
  gap: 10px;
}

.paginationButton {
  background: linear-gradient(135deg, #4a90e2, #357ae8);
  color: white;
  border: none;
  padding: 8px 12px;
  margin: 0 5px;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
}

.paginationButton:disabled {
  background: #e0e0e0;
  cursor: not-allowed;
  opacity: 0.6;
}

.paginationButton:hover:not(:disabled) {
  transform: scale(1.05);
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.activePage {
  background: linear-gradient(135deg, #50c878, #4caf50);
  transform: scale(1.1);
}
