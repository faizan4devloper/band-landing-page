
import React, { useEffect, useState } from "react";
import axios from "axios";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faSyncAlt } from "@fortawesome/free-solid-svg-icons";
import { HashLoader } from "react-spinners";
import styles from "./AllDataTable.module.css";

const AllDataTable = () => {
  const [rows, setRows] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState("");
  const [spinningRows, setSpinningRows] = useState({});

  const fetchData = async () => {
    setLoading(true);
    setError("");
    try {
      const payload = {
        tasktype: "FETCH_ALL_ACT_CLAIMS", // Different payload
      };

      const headers = {
        "Content-Type": "application/json",
      };

      const response = await axios.post("dummy", payload, { headers }); // Different API URL

      console.log("API Response:", response.data);

      const allData = Object.values(response.data.alldata || {});
      console.log("Extracted All Data:", allData);

      setRows(allData);
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
              <th>Column 1</th>
              <th>Column 2</th>
              <th>Column 3</th>
              <th>Column 4</th>
              <th>Column 5</th>
              <th>Actions</th>
            </tr>
          </thead>
          <tbody>
            {rows.map((row, index) => {
              const uniqueKey = `${index}_${row.uniqueField}`; // Adjust unique key based on available data
              return (
                <tr key={uniqueKey}>
                  <td>{row.claimid}</td>
                  <td>{row.briefsummary}</td>
                  <td>{row.claimtype}</td>
                  <td>{row.status}</td>
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

export default AllDataTable;
