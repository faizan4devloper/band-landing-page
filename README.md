import React, { useEffect, useState } from "react";
import axios from "axios";
import { HashLoader } from "react-spinners";
import Modal from 'react-modal';
import styles from "./AllDataTable.module.css";

Modal.setAppElement('#root');

const AllDataTable = () => {
  const [rows, setRows] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState("");
  const [selectedRow, setSelectedRow] = useState(null);
  const [isModalOpen, setIsModalOpen] = useState(false);

  // Pagination State
  const [currentPage, setCurrentPage] = useState(1);
  const itemsPerPage = 5; // Display 5 items per page

  const fetchData = async () => {
    setLoading(true);
    setError("");
    try {
      const payload = {
        tasktype: "FETCH_ALL_ACT_CLAIMS",
      };

      const headers = {
        "Content-Type": "application/json",
      };

      const response = await axios.post("dummy", payload, { headers });
      console.log("API Response:", response.data);

      const allData = Object.values(response.data.allclaimactdata || {});
      setRows(allData);
    } catch (err) {
      setError("Failed to fetch data. Please try again.");
      console.error("API Error:", err);
    } finally {
      setLoading(false);
    }
  };

  useEffect(() => {
    fetchData();
  }, []);

  // Pagination Logic
  const indexOfLastItem = currentPage * itemsPerPage;
  const indexOfFirstItem = indexOfLastItem - itemsPerPage;
  const currentItems = rows.slice(indexOfFirstItem, indexOfLastItem);
  const totalPages = Math.ceil(rows.length / itemsPerPage);

  // Page Change Handlers
  const paginate = (pageNumber) => setCurrentPage(pageNumber);
  const nextPage = () => setCurrentPage((prev) => Math.min(prev + 1, totalPages));
  const prevPage = () => setCurrentPage((prev) => Math.max(prev - 1, 1));

  return (
    <div className={styles.tableContainer}>
      {loading ? (
        <div className={styles.spinnerContainer}>
          <HashLoader color="#0f5fdc" size={40} />
        </div>
      ) : error ? (
        <p className={styles.error}>{error}</p>
      ) : currentItems.length > 0 ? (
        <>
          <table className={styles.table}>
            <thead>
              <tr>
                <th>Claim ID</th>
                <th>Summary</th>
                <th>Claim Type</th>
                <th>Status</th>
              </tr>
            </thead>
            <tbody>
              {currentItems.map((row, index) => (
                <tr key={index} onClick={() => setSelectedRow(row) & setIsModalOpen(true)}>
                  <td className={styles.clickable}>{row.claimid}</td>
                  <td>{row.briefsummary}</td>
                  <td>{row.claimtype}</td>
                  <td>{row.status}</td>
                </tr>
              ))}
            </tbody>
          </table>

          {/* Pagination Controls */}
          <div className={styles.pagination}>
            <button onClick={prevPage} disabled={currentPage === 1} className={styles.pageButton}>
              ◀ Prev
            </button>
            {[...Array(totalPages)].map((_, i) => (
              <button key={i} onClick={() => paginate(i + 1)}
                className={`${styles.pageButton} ${currentPage === i + 1 ? styles.activePage : ""}`}>
                {i + 1}
              </button>
            ))}
            <button onClick={nextPage} disabled={currentPage === totalPages} className={styles.pageButton}>
              Next ▶
            </button>
          </div>
        </>
      ) : (
        <p className={styles.noData}>No data available</p>
      )}

      {/* Modal */}
      <Modal isOpen={isModalOpen} onRequestClose={() => setIsModalOpen(false)}
        className={styles.modal} overlayClassName={styles.modalOverlay}>
        {selectedRow && (
          <div className={styles.modalContent}>
            <h2 className={styles.modalTitle}>{selectedRow.claimid} Details</h2>
            <p><strong>Brief Summary:</strong> {selectedRow.briefsummary}</p>
            <p><strong>Claim Type:</strong> {selectedRow.claimtype}</p>
            <p><strong>Claim Status:</strong> {selectedRow.status}</p>
          </div>
        )}
        <button onClick={() => setIsModalOpen(false)} className={styles.modalCloseButton}>Close</button>
      </Modal>
    </div>
  );
};

export default AllDataTable;


/* Pagination Container */
.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 15px;
}

/* Pagination Buttons */
.pageButton {
  background-color: #4a90e2;
  color: white;
  border: none;
  padding: 8px 12px;
  margin: 0 5px;
  cursor: pointer;
  border-radius: 4px;
  font-size: 14px;
  transition: background 0.3s ease-in-out;
}

.pageButton:hover {
  background-color: #357ae8;
}

.pageButton:disabled {
  background-color: #ddd;
  cursor: not-allowed;
}

/* Active Page Styling */
.activePage {
  background-color: #0f5fdc;
  font-weight: bold;
}
