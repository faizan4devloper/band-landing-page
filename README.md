import React, { useState } from "react";
import styles from "./ProductSheetsPage.module.css";
import axios from "axios";

const ProductSheetsPage = () => {
  const [file, setFile] = useState(null);
  const [message, setMessage] = useState("");
  const [tableData, setTableData] = useState([]);

  const handleFileChange = (event) => {
    setFile(event.target.files[0]);
  };

  const handleUpload = async () => {
    if (!file) {
      setMessage("Please select a file to upload.");
      return;
    }

    try {
      setMessage("Uploading...");
      const payload = {
        payload: {
          filename: file.name,
        },
      };

      const { data } = await axios.post(
        "https://<your-api-gateway-url>",
        payload,
        {
          headers: { "Content-Type": "application/json" },
        }
      );

      const url = data.presignedUrl;
      if (!url) {
        throw new Error("Pre-signed URL not received from API");
      }

      await axios.put(url, file, {
        headers: { "Content-Type": file.type },
      });

      // Update the table with new data
      setTableData((prevData) => [
        ...prevData,
        {
          policyId: `PID${Math.floor(Math.random() * 1000)}`,
          productSheetType: "PDF",
          summary: "File uploaded successfully",
          previewLink: url.split("?")[0], // Strip query params
        },
      ]);

      setMessage("File uploaded successfully!");
      setFile(null);
    } catch (error) {
      console.error("Upload failed:", error.message);
      setMessage("Failed to upload the file.");
    }
  };

  return (
    <div className={styles.container}>
      {/* Sidebar */}
      <aside className={styles.sidebar}>
        <h2>Manage Product Sheets</h2>
        <input
          type="file"
          onChange={handleFileChange}
          className={styles.fileInput}
        />
        <button onClick={handleUpload} className={styles.uploadButton}>
          Upload and Process
        </button>
        {message && <p className={styles.message}>{message}</p>}
      </aside>

      {/* Main Content */}
      <main className={styles.mainContent}>
        <h2>Product Sheets Table</h2>
        <table className={styles.table}>
          <thead>
            <tr>
              <th>Policy ID</th>
              <th>Product Sheet Type</th>
              <th>Summary</th>
              <th>Preview</th>
            </tr>
          </thead>
          <tbody>
            {tableData.map((row, index) => (
              <tr key={index}>
                <td>{row.policyId}</td>
                <td>{row.productSheetType}</td>
                <td>{row.summary}</td>
                <td>
                  <a href={row.previewLink} target="_blank" rel="noopener noreferrer">
                    Preview
                  </a>
                </td>
              </tr>
            ))}
          </tbody>
        </table>
      </main>
    </div>
  );
};

export default ProductSheetsPage;


.container {
  display: flex;
  height: 100vh;
  font-family: Arial, sans-serif;
  background-color: #f5f7fa;
}

/* Sidebar Styles */
.sidebar {
  width: 25%;
  padding: 20px;
  background-color: #3f51b5;
  color: white;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.sidebar h2 {
  font-size: 1.5rem;
  margin-bottom: 20px;
}

.fileInput {
  margin-bottom: 15px;
  padding: 8px;
  border: none;
  border-radius: 4px;
}

.uploadButton {
  padding: 10px 20px;
  background-color: #ffffff;
  color: #3f51b5;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: bold;
}

.uploadButton:hover {
  background-color: #f0f0f0;
}

.message {
  margin-top: 10px;
  font-size: 0.9rem;
  color: yellow;
  text-align: center;
}

/* Main Content Styles */
.mainContent {
  flex: 1;
  padding: 20px;
}

.mainContent h2 {
  margin-bottom: 20px;
  color: #3f51b5;
}

/* Table Styles */
.table {
  width: 100%;
  border-collapse: collapse;
  background-color: white;
}

.table th, .table td {
  padding: 10px 15px;
  text-align: left;
  border: 1px solid #ddd;
}

.table th {
  background-color: #f5f5f5;
  font-weight: bold;
}

.table tr:nth-child(even) {
  background-color: #f9f9f9;
}

.table tr:hover {
  background-color: #f1f1f1;
}

.table a {
  color: #3f51b5;
  text-decoration: none;
}

.table a:hover {
  text-decoration: underline;
}
