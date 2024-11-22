import React, { useState, useEffect } from "react";
import axios from "axios";
import Sidebar from "./Sidebar";
import MainContent from "./MainContent";
import DataTable from "./DataTable"; // Create this component
import styles from "./ProductSheetsPage.module.css";

const ProductSheetsPage = () => {
  const [allClaimData, setAllClaimData] = useState([]);
  const [showProcessing, setShowProcessing] = useState(false);

  // Fetch data on component mount
  useEffect(() => {
    const fetchClaimData = async () => {
      try {
        const response = await axios.get("your-api-endpoint"); // Replace with your API endpoint
        setAllClaimData(response.data);
      } catch (error) {
        console.error("Error fetching claim data:", error);
      }
    };

    fetchClaimData();
  }, []);

  return (
    <div className={styles.container}>
      {!showProcessing ? (
        <div>
          <h1>Product Sheets</h1>
          <DataTable data={allClaimData} />
          <button
            className={styles.processingButton}
            onClick={() => setShowProcessing(true)}
          >
            Processing
          </button>
        </div>
      ) : (
        <div className={styles.processingContainer}>
          <Sidebar />
          <MainContent />
        </div>
      )}
    </div>
  );
};

export default ProductSheetsPage;



import React from "react";
import styles from "./DataTable.module.css";

const DataTable = ({ data }) => {
  if (!data.length) return <p>No data available</p>;

  return (
    <table className={styles.table}>
      <thead>
        <tr>
          <th>Policy ID</th>
          <th>Type</th>
          <th>Summary</th>
          <th>File Name</th>
          <th>Record Number</th>
        </tr>
      </thead>
      <tbody>
        {data.map((item, index) => (
          <tr key={index}>
            <td>{item.policy_id}</td>
            <td>{item.prod_sheet_type}</td>
            <td>{item.summary}</td>
            <td>{item.file_name}</td>
            <td>{item.rec_number}</td>
          </tr>
        ))}
      </tbody>
    </table>
  );
};

export default DataTable;




.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 20px;
}

.processingButton {
  margin-top: 20px;
  padding: 10px 20px;
  font-size: 16px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.processingButton:hover {
  background-color: #0056b3;
}

.processingContainer {
  display: flex;
}





.table {
  width: 100%;
  border-collapse: collapse;
  margin: 20px 0;
}

.table th,
.table td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

.table th {
  background-color: #f2f2f2;
  font-weight: bold;
}
