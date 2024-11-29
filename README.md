import React, { useEffect, useState } from "react";
import axios from "axios";
import Modal from "react-modal"; // Import the modal
import { HashLoader } from "react-spinners";
import styles from "./DataTable.module.css";

Modal.setAppElement("#root"); // Accessibility requirement for react-modal

const DataTable = () => {
  const [rows, setRows] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState("");
  const [selectedRow, setSelectedRow] = useState(null); // State to track selected row
  const [isModalOpen, setIsModalOpen] = useState(false); // State to track modal visibility

  const fetchData = async () => {
    setLoading(true);
    setError("");
    try {
      const payload = {
        tasktype: "FETCH_ALL_CLAIMS",
      };

      const headers = {
        "Content-Type": "application/json",
      };

      const response = await axios.post("dummy", payload, { headers });

      console.log("API Response:", response.data);

      const claimData = Object.values(response.data.allclaimdata || {});
      console.log("Extracted Claim Data:", claimData);

      setRows(claimData);
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
              <th>File Name</th>
              <th>Rec No.</th>
              <th nowrap width="10%">Policy ID</th>
              <th>Type</th>
              <th>Summary</th>
              <th>Status</th>
            </tr>
          </thead>
          <tbody>
            {rows.map((row, index) => {
              const uniqueKey = `${index}_${row.rec_number}`; // Create a unique key
              return (
                <tr key={uniqueKey}>
                  <td>{row.file_name}</td>
                  <td
                    className={styles.clickable}
                    onClick={() => openModal(row)} // Open modal on click
                  >
                    {row.rec_number}
                  </td>
                  <td>{row.policy_id}</td>
                  <td>{row.prod_sheet_type}</td>
                  <td>{row.summary}</td>
                  <td>{row.status || "Pending"}</td>
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
        <h2 className={styles.modalTitle}>Row Details</h2>
        {selectedRow && (
          <div className={styles.modalContent}>
            <p>
              <strong>File Name:</strong> {selectedRow.file_name}
            </p>
            <p>
              <strong>Rec No.:</strong> {selectedRow.rec_number}
            </p>
            <p>
              <strong>Policy ID:</strong> {selectedRow.policy_id}
            </p>
            <p>
              <strong>Type:</strong> {selectedRow.prod_sheet_type}
            </p>
            <p>
              <strong>Summary:</strong> {selectedRow.summary}
            </p>
            <p>
              <strong>Status:</strong> {selectedRow.status || "Pending"}
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

export default DataTable;












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
