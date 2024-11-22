import React, { useState, useEffect } from "react";
import axios from "axios";
import AllDataTable from "./AllDataTable";

const ManageClaims = () => {
  const [claimsData, setClaimsData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await axios.post("dummy1", {
          tasktype: "FETCH_ALL_ACT_CLAIMS",
        });

        console.log(response.data); // Debugging: Check the API response

        // Validate and set the data
        if (response.data.allclaimactdata) {
          setClaimsData(response.data.allclaimactdata);
        } else {
          throw new Error("Invalid API response structure");
        }
      } catch (err) {
        console.error(err);
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

const AllDataTable = ({ data = [] }) => {
  if (!data.length) {
    return <p>No claims data available.</p>;
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
            <td>{claim.briefsummary}</td>
            <td>{claim.status}</td>
          </tr>
        ))}
      </tbody>
    </table>
  );
};

export default AllDataTable;
