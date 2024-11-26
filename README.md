import React, { useEffect, useState } from "react";
import axios from "axios";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faSyncAlt } from "@fortawesome/free-solid-svg-icons";
import { HashLoader } from "react-spinners";
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
        <div className={styles.spinnerContainer}>
          <HashLoader color="#0f5fdc" size={40} />
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
                        className={`${styles.reloadIcon} ${
                          spinningRows[uniqueKey] ? "fa-spin" : ""
                        }`}
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
