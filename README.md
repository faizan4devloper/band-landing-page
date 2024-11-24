import React, { useEffect, useState } from 'react';
import axios from 'axios';
import styles from './DataTable.module.css';

const DataTable = () => {
  const [rows, setRows] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState("");

  // Fetch all data initially
  const fetchData = async () => {
    setLoading(true);
    setError(""); // Clear any previous errors
    try {
      const payload = {
        tasktype: "FETCH_ALL_CLAIMS",
      };

      const headers = {
        "Content-Type": "application/json",
        Authorization: "Bearer your-auth-token-here",
      };

      const response = await axios.post("https://your-api-endpoint.com/fetchClaims", payload, {
        headers,
      });

      console.log("API Response:", response.data);

      // Assuming the data is an array of objects
      setRows(response.data.claims || []); // Adjust based on API response structure
    } catch (err) {
      setError("Failed to fetch data. Please try again.");
      console.error("API Error:", err);
    } finally {
      setLoading(false);
    }
  };

  // Reload a single row's data
  const handleReload = async (recNum) => {
    try {
      const payload = {
        tasktype: "FETCH_SINGLE_CLAIM",
        claimid: recNum,
      };

      const headers = {
        "Content-Type": "application/json",
        Authorization: "Bearer your-auth-token-here",
      };

      const response = await axios.post("https://your-api-endpoint.com/fetchSingleClaim", payload, {
        headers,
      });

      console.log("Reload Response for RecNum:", recNum, response.data);

      const updatedData = response.data;

      // Update the specific row in the table
      setRows((prevRows) =>
        prevRows.map((row) =>
          row.recNum === recNum
            ? {
                ...row,
                policyid: updatedData.policyid,
                type: updatedData.type,
                summary: updatedData.summary,
                status: updatedData.status || "Updated",
              }
            : row
        )
      );
    } catch (error) {
      console.error("Reload Error:", error);
      setError(`Failed to reload data for RecNum: ${recNum}`);
    }
  };

  // Fetch all data on mount
  useEffect(() => {
    fetchData();
  }, []);

  return (
    <div className={styles.tableContainer}>
      {loading ? (
        <p>Loading data...</p>
      ) : error ? (
        <p className={styles.error}>{error}</p>
      ) : rows.length > 0 ? (
        <table className={styles.table}>
          <thead>
            <tr>
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
                <td>{row.recNum}</td>
                <td>{row.policyid}</td>
                <td>{row.type}</td>
                <td>{row.summary}</td>
                <td>{row.status}</td>
                <td>
                  <button
                    className={styles.reloadButton}
                    onClick={() => handleReload(row.recNum)}
                  >
                    Reload
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
