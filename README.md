import React, { useEffect, useState } from "react";
import axios from "axios";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faSyncAlt } from "@fortawesome/free-solid-svg-icons";
import styles from "./DataTable.module.css";

const DataTable = () => {
  const [rows, setRows] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState("");
  const [spinningRows, setSpinningRows] = useState({});

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
            {rows.map((row, index) => {
              const uniqueKey = `${index}_${row.rec_number}`; // Create a unique key
              return (
                <tr key={uniqueKey}>
                  <td>{row.file_name}</td>
                  <td>{row.rec_number}</td>
                  <td>{row.policy_id}</td>
                  <td>{row.prod_sheet_type}</td>
                  <td>{row.summary}</td>
                  <td>{row.status || "Pending"}</td>
                  <td>
                    <button
                      className={styles.reloadButton}
                      onClick={() => handleReloadRow(uniqueKey)}
                    >
                      <FontAwesomeIcon
                        icon={faSyncAlt}
                        className={spinningRows[uniqueKey] ? "fa-spin" : ""}
                      />
                    </button>
                  </td>
                </tr>
              );
            })}
          </tbody>
        </table>
      ) : (
        <p className={styles.noData}>No data available</p>
      )}
    </div>
  );
};

export default DataTable;



/* Container for the table */
.tableContainer {
  margin-top: 20px;
  overflow-x: auto;
  border-radius: 10px;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
  background-color: #fff;
  padding: 20px;
}

/* Table styling */
.table {
  width: 100%;
  border-collapse: collapse;
  font-family: "Roboto", sans-serif;
  font-size: 16px;
  color: #333;
}

/* Header styling */
.table th {
  background: linear-gradient(90deg, #4caf50, #66bb6a);
  color: white;
  text-align: left;
  padding: 14px;
  font-weight: bold;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

/* Cell styling */
.table td {
  border: 1px solid #ddd;
  padding: 12px;
  text-align: left;
  color: #555;
}

/* Alternate row color for zebra effect */
.table tr:nth-child(even) {
  background-color: #f9f9f9;
}

/* Hover effect */
.table tr:hover {
  background-color: #e8f5e9;
  transition: background-color 0.3s ease-in-out;
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

/* Reload icon */
.reloadIcon {
  font-size: 16px;
  margin-left: 8px;
}

/* Spinning animation */
.spinning {
  animation: spin 1s linear infinite;
}

@keyframes spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
