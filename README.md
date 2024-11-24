import React from "react";
import styles from "./ManageClaims.module.css";
import AllDataTable from "./AllDataTable";
import { useNavigate } from "react-router-dom";

const ManageClaims = () => {
  const navigate = useNavigate();

  return (
    <div className={styles.manageClaimsContainer}>
      <header className={styles.header}>
        <h1 className={styles.title}>Manage Claims</h1>
        <button
          className={styles.newClaimButton}
          onClick={() => navigate("/new-claim")}
        >
          New Claim Processing
        </button>
      </header>
      <AllDataTable />
    </div>
  );
};

export default ManageClaims;



.manageClaimsContainer {
  display: flex;
  flex-direction: column;
  padding: 20px;
  background-color: #f8f9fa;
  height: 100vh;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  background-color: #fff;
  padding: 10px 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.title {
  font-size: 1.5rem;
  font-weight: bold;
  color: #343a40;
}

.newClaimButton {
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  padding: 10px 15px;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.newClaimButton:hover {
  background-color: #0056b3;
}




import React, { useEffect, useState } from "react";
import axios from "axios";
import styles from "./AllDataTable.module.css";

const AllDataTable = () => {
  const [data, setData] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    axios.get("https://api.example.com/claims") // Replace with your API URL
      .then((response) => {
        setData(response.data);
        setLoading(false);
      })
      .catch((error) => {
        console.error("Error fetching data:", error);
        setLoading(false);
      });
  }, []);

  if (loading) return <div className={styles.loader}>Loading...</div>;

  return (
    <table className={styles.table}>
      <thead>
        <tr>
          <th>ID</th>
          <th>Claimant Name</th>
          <th>Status</th>
          <th>Date Submitted</th>
        </tr>
      </thead>
      <tbody>
        {data.map((claim) => (
          <tr key={claim.id}>
            <td>{claim.id}</td>
            <td>{claim.claimantName}</td>
            <td>{claim.status}</td>
            <td>{claim.dateSubmitted}</td>
          </tr>
        ))}
      </tbody>
    </table>
  );
};

export default AllDataTable;



.table {
  width: 100%;
  border-collapse: collapse;
  background-color: #fff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.table th, .table td {
  padding: 10px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

.table th {
  background-color: #007bff;
  color: #fff;
}

.table tr:hover {
  background-color: #f1f1f1;
}




import React from "react";
import styles from "./NewClaimPage.module.css";

const NewClaimPage = () => {
  return (
    <div className={styles.newClaimPage}>
      <aside className={styles.sidebar}>
        <nav>
          <ul>
            <li>Option 1</li>
            <li>Option 2</li>
            <li>Option 3</li>
          </ul>
        </nav>
      </aside>
      <main className={styles.mainContent}>
        <h2>New Claim Processing</h2>
        {/* Add your form or content here */}
      </main>
    </div>
  );
};

export default NewClaimPage;




.newClaimPage {
  display: flex;
  height: 100vh;
}

.sidebar {
  width: 20%;
  background-color: #343a40;
  color: #fff;
  padding: 20px;
}

.sidebar ul {
  list-style: none;
  padding: 0;
}

.sidebar li {
  margin-bottom: 10px;
  cursor: pointer;
}

.sidebar li:hover {
  text-decoration: underline;
}

.mainContent {
  width: 80%;
  padding: 20px;
  background-color: #f8f9fa;
}




import React from "react";
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";
import ManageClaims from "./ManageClaims";
import NewClaimPage from "./NewClaimPage";

const App = () => {
  return (
    <Router>
      <Routes>
        <Route path="/" element={<ManageClaims />} />
        <Route path="/new-claim" element={<NewClaimPage />} />
      </Routes>
    </Router>
  );
};

export default App;
