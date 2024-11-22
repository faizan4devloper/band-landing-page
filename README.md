import React, { useState, useEffect } from "react";
import axios from "axios";
import AllDataTable from "./AllDataTable";

const ManageClaims = () => {
  const [claimsData, setClaimsData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    // Fetch data from API
    const fetchData = async () => {
      try {
        const response = await axios.get("YOUR_API_ENDPOINT"); // Replace with your API URL
        setClaimsData(response.data.payload); // Assuming 'payload' contains the array of data
      } catch (err) {
        setError("Failed to fetch claims data.");
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, []);

  if (loading) return <p>Loading...</p>;
  if (error) return <p>{error}</p>;

  return (
    <div className="manage-claims">
      <h1>Manage Claims</h1>
      <AllDataTable data={claimsData} />
    </div>
  );
};

export default ManageClaims;


import React from "react";
import styles from "./AllDataTable.module.css";

const AllDataTable = ({ data }) => {
  return (
    <div className={styles.tableContainer}>
      <table className={styles.table}>
        <thead>
          <tr>
            <th className={styles.headerCell}>Claim ID</th>
            <th className={styles.headerCell}>Claim Type</th>
            <th className={styles.headerCell}>Summary</th>
            <th className={styles.headerCell}>Status</th>
          </tr>
        </thead>
        <tbody>
          {data.map((claim, index) => (
            <tr
              key={index}
              className={`${index % 2 === 0 ? styles.rowEven : ""} ${styles.rowHover}`}
            >
              <td className={styles.rowCell}>{claim.claimid}</td>
              <td className={styles.rowCell}>{claim.claimtype}</td>
              <td className={styles.rowCell}>{claim.summary}</td>
              <td className={styles.rowCell}>{claim.status}</td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
};

export default AllDataTable;


.tableContainer {
  width: 100%;
  overflow-x: auto;
  margin-top: 20px;
}

.table {
  width: 100%;
  border-collapse: collapse;
  margin: 20px 0;
  font-size: 16px;
  text-align: left;
}

.headerCell {
  padding: 12px 15px;
  background-color: #007bff;
  color: #ffffff;
  border: 1px solid #ddd;
}

.rowCell {
  padding: 12px 15px;
  border: 1px solid #ddd;
  color: #333333;
}

.rowEven {
  background-color: #f9f9f9;
}

.rowHover:hover {
  background-color: #f1f1f1;
  cursor: pointer;
}

