import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faRotateRight, faSpinner } from '@fortawesome/free-solid-svg-icons';

const MainContent = () => {
  const [rows, setRows] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState("");
  const [spinningRows, setSpinningRows] = useState({});

  // Fetch all claim data
  const fetchData = async () => {
    setLoading(true);
    setError("");

    try {
      const payload = { tasktype: "FETCH_ALL_CLAIMS" };
      const headers = { "Content-Type": "application/json" };

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

  // Reload specific row data
  const handleReloadRow = async (uniqueKey) => {
    setSpinningRows((prev) => ({ ...prev, [uniqueKey]: true }));

    try {
      console.log(`Reloading data for uniqueKey: ${uniqueKey}`);
      // Simulate API call delay
      await new Promise((resolve) => setTimeout(resolve, 1000));
      // Optionally update the row data here after fetching the updated information
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
    <div className={styles.mainContent}>
      {loading ? (
        <p>Loading...</p>
      ) : error ? (
        <p>{error}</p>
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
              const uniqueKey = `${index}_${row.recNum}`; // Unique identifier for each row

              return (
                <tr key={uniqueKey}>
                  <td>{row.recNum}</td>
                  <td>{row.policy_id || "Pending"}</td>
                  <td>{row.prod_sheet_type || "Pending"}</td>
                  <td>{row.summary || "Pending"}</td>
                  <td>
                    {row.previewLink ? (
                      <a href={row.previewLink} target="_blank" rel="noopener noreferrer">
                        View Preview
                      </a>
                    ) : (
                      "Pending"
                    )}
                  </td>
                  <td>{row.status || "Pending"}</td>
                  <td>
                    <button
                      onClick={() => handleReloadRow(uniqueKey)}
                      disabled={spinningRows[uniqueKey]}
                      className={styles.reloadButton}
                    >
                      {spinningRows[uniqueKey] ? (
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
        <p>No data available</p>
      )}
    </div>
  );
};

export default MainContent;
