import React, { useState, useEffect } from "react";
import axios from "axios";
import styles from "./ManageClaims.module.css";

const ManageClaims = () => {
  const [claims, setClaims] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  const fetchClaims = async () => {
    try {
      setLoading(true);
      const response = await axios.post("YOUR_API_URL", {
        tasktype: "FETCH_ALL_ACT_CLAIMS",
      });
      setClaims(response.data); // Assuming the response contains an array of claims
    } catch (err) {
      setError("Failed to fetch claims. Please try again later.");
      console.error(err);
    } finally {
      setLoading(false);
    }
  };

  const handleStatusChange = (id, newStatus) => {
    setClaims((prevClaims) =>
      prevClaims.map((claim) =>
        claim.id === id ? { ...claim, status: newStatus } : claim
      )
    );
  };

  useEffect(() => {
    fetchClaims();
  }, []);

  if (loading) return <div className={styles.loading}>Loading claims...</div>;
  if (error) return <div className={styles.error}>{error}</div>;

  return (
    <div className={styles.container}>
      <h1 className={styles.heading}>Manage Claims</h1>
      <table className={styles.table}>
        <thead>
          <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Claim Type</th>
            <th>Status</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          {claims.map((claim) => (
            <tr key={claim.id}>
              <td>{claim.id}</td>
              <td>{claim.name}</td>
              <td>{claim.claimType}</td>
              <td className={`${styles.status} ${styles[claim.status.toLowerCase()]}`}>
                {claim.status}
              </td>
              <td>
                <select
                  value={claim.status}
                  onChange={(e) => handleStatusChange(claim.id, e.target.value)}
                  className={styles.dropdown}
                >
                  <option value="Pending">Pending</option>
                  <option value="Approved">Approved</option>
                  <option value="Rejected">Rejected</option>
                </select>
              </td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
};

export default ManageClaims;



.container {
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.heading {
  text-align: center;
  margin-bottom: 20px;
  color: #333;
  font-size: 24px;
}

.table {
  width: 100%;
  border-collapse: collapse;
  background-color: #fff;
  margin: 0 auto;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.table th,
.table td {
  text-align: left;
  padding: 12px 16px;
  border-bottom: 1px solid #ddd;
}

.table th {
  background-color: #0078d4;
  color: #fff;
  font-weight: bold;
}

.table tr:hover {
  background-color: #f1f1f1;
}

.status {
  font-weight: bold;
}

.status.pending {
  color: #f39c12;
}

.status.approved {
  color: #27ae60;
}

.status.rejected {
  color: #e74c3c;
}

.dropdown {
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  outline: none;
  font-size: 14px;
}

.loading {
  text-align: center;
  font-size: 18px;
  color: #0078d4;
}

.error {
  text-align: center;
  font-size: 16px;
  color: #e74c3c;
}
