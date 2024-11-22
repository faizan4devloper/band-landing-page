import React from "react";
import styles from "./AllDataTable.module.css";

const AllDataTable = ({ data = [] }) => {
  if (!data.length) {
    return <p>No claims data available.</p>; // Show a message for empty data
  }

  return (
    <table className={styles.table}>
      <thead>
        <tr>
          <th>Claim ID</th>
          <th>Claim Type</th>
          <th>Summary</th>
          <th>Status</th>
        </tr>
      </thead>
      <tbody>
        {data.map((claim) => (
          <tr key={claim.claimid}>
            <td>{claim.claimid}</td>
            <td>{claim.claimtype}</td>
            <td>{claim.summary}</td>
            <td>{claim.status}</td>
          </tr>
        ))}
      </tbody>
    </table>
  );
};

export default AllDataTable;
