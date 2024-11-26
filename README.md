import React, { useEffect, useState } from 'react';
import axios from 'axios';
import Modal from 'react-modal';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faRotateRight, faSpinner } from '@fortawesome/free-solid-svg-icons';
import styles from './MainContent.module.css';

const MainContent = ({ message }) => {
  const [rows, setRows] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState("");
  const [loadingRows, setLoadingRows] = useState([]);

  // Fetch data on component mount
  const fetchData = async () => {
    setLoading(true);
    setError("");

    try {
      const payload = { tasktype: "FETCH_SINGLE_CLAIM" };
      const headers = { "Content-Type": "application/json" };

      const response = await axios.post("dummy", payload, { headers });

      console.log("API Response:", response.data);

      // Extracting claim data similarly to DataTable
      const claimData = Object.values(response.data.allclaimdata || {});
      console.log("Extracted Claim Data:", claimData);

      // Set rows with the extracted data
      setRows(claimData);
    } catch (err) {
      setError("Failed to fetch data. Please try again.");
      console.error("API Error:", err);
    } finally {
      setLoading(false);
    }
  };

  // Reload data for a specific row
  const handleReload = async (recNum) => {
    setLoadingRows((prev) => [...prev, recNum]); // Add to loading state
    try {
      const payload = {
        tasktype: "FETCH_SINGLE_CLAIM",
        claimid: recNum,
      };

      const response = await axios.post("dummy", payload, {
        headers: { "Content-Type": "application/json" },
      });

      const singleClaimData = response.data;
      console.log("Reload Data:", response.data);

      // Update rows with reloaded data
      setRows((prevRows) =>
        prevRows.map((row) =>
          row.recNum === recNum
            ? {
                ...row,
                policy_id: singleClaimData.policy_id,
                prod_sheet_type: singleClaimData.prod_sheet_type,
                summary: singleClaimData.summary,
                status: singleClaimData.status,
                previewLink: singleClaimData.previewLink,
              }
            : row
        )
      );
    } catch (error) {
      console.error("Failed to fetch data for RecNum:", recNum, error);
    } finally {
      setLoadingRows((prev) => prev.filter((id) => id !== recNum)); // Remove from loading state
    }
  };

  // Fetch data when the component mounts
  useEffect(() => {
    fetchData();
  }, []);

  return (
    <div className={styles.mainContent}>
      {loading ? (
        <p>Loading...</p> // Show loading message when fetching data
      ) : error ? (
        <p>{error}</p> // Display error message
      ) : rows.length > 0 ? (
        <table className={styles.table}>
          <thead>
            <tr>
              <th>RecNum</th>
              <th>Policy ID</th>
              <th>Product Sheet Type</th>
              <th>Summary</th>
              <th>Preview Link</th>
              <th>Status</th>
              <th>Actions</th>
            </tr>
          </thead>
          <tbody>
            {rows.map((row, index) => {
              const uniqueKey = `${index}_${row.recNum}`; // Unique key for each row
              return (
                <tr key={uniqueKey}>
                  <td>{row.recNum}</td>
                  <td>{row.policy_id || "Pending"}</td>
                  <td>{row.prod_sheet_type || "Pending"}</td>
                  <td>{row.summary || "Pending"}</td>
                  <td>
                    {row.previewLink ? (
                      <button onClick={() => openModal(row.previewLink)}>
                        Preview
                      </button>
                    ) : (
                      "Pending"
                    )}
                  </td>
                  <td>
                    <button
                      onClick={() => handleReload(row.recNum)}
                      disabled={loadingRows.includes(row.recNum)}
                    >
                      {loadingRows.includes(row.recNum) ? (
                        <FontAwesomeIcon icon={faSpinner} spin />
                      ) : (
                        <FontAwesomeIcon icon={faRotateRight} />
                      )}
                    </button>
                  </td>
                </tr>
              );
            })}
          </tbody>
        </table>
      ) : (
        <p>No data available</p> // Message if no data is present
      )}
    </div>
  );
};

export default MainContent;
