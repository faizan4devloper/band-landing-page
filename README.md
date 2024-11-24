import React, { useEffect, useState } from "react";
import axios from "axios";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faSyncAlt } from "@fortawesome/free-solid-svg-icons";
import styles from "./DataTable.module.css";

const DataTable = () => {
  const [rows, setRows] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState("");
  const [spinningRow, setSpinningRow] = useState(null); // Track which row's icon is spinning

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

      // Extract values from the object
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

  const handleReloadRow = async (recNumber) => {
    setSpinningRow(recNumber); // Set the spinning row
    try {
      console.log(`Reloading data for RecNum: ${recNumber}`);
      // Simulate reload logic (e.g., refreshing row data)
      await new Promise((resolve) => setTimeout(resolve, 1000)); // Simulated delay
    } catch (err) {
      console.error("Error reloading row:", err);
    } finally {
      setSpinningRow(null); // Stop spinning after the reload
    }
  };

  useEffect(() => {
    fetchData();
  }, []);

  return (
    <div className={styles.tableContainer}>
      {loading ? (
        <div style={{ display: "flex", justifyContent: "center", marginTop: "50px" }}>
          <p>Loading data...</p>
        </div>
      ) : error ? (
        <p className={styles.error}>{error}</p>
      ) : rows.length > 0 ? (
        <table className={styles.table}>
          <thead>
            <tr>
              <th>FileName</th>
              <th>RecNum</th>
              <th>Policy ID</th>
              <th>Type</th>
              <th>Summary</th>
              <th>Status</th>
              <th>Actions</th>
            </tr>
          </thead>
          <tbody>
            {rows.map((row, index) => (
              <tr key={index}>
                <td>{row.file_name}</td>
                <td>{row.rec_number}</td>
                <td>{row.policy_id}</td>
                <td>{row.prod_sheet_type}</td>
                <td>{row.summary}</td>
                <td>{row.status || "Pending"}</td>
                <td>
                  <button
                    className={styles.reloadButton}
                    onClick={() => handleReloadRow(row.rec_number)}
                  >
                    <FontAwesomeIcon
                      icon={faSyncAlt}
                      className={spinningRow === row.rec_number ? "fa-spin" : ""}
                    />
                  </button>
                </td>
              </tr>
            ))}
          </tbody>
        </table>
      ) : (
        <p className={styles.noData}>No data available</p>
      )}
    </div>
  );
};

export default DataTable;



/* Reload button styling */
.reloadButton {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 8px;
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

/* FontAwesome icon spinning animation is handled by "fa-spin" class */
