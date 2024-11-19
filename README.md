import React from 'react';
import styles from './MainContent.module.css'; // Custom CSS for MainContent

const MainContent = ({ message, rows, handleReload }) => {
  return (
    <div className={styles.mainContent}>
      <h2>Uploaded Product Sheets</h2>
      {message && <p>{message}</p>}
      
      {/* Table to display uploaded data */}
      <table className={styles.table}>
        <thead>
          <tr>
            <th>RecNum</th>
            <th>Policy ID</th>
            <th>Product Sheet Type</th>
            <th>Summary</th>
            <th>Preview Link</th>
            <th>Status</th>
          </tr>
        </thead>
        <tbody>
          {rows && rows.length > 0 ? (
            rows.map((row, index) => (
              <tr key={index}>
                <td>{row.recNum}</td>
                <td>{row.policyId || "Loading..."}</td>
                <td>{row.productSheetType || "Loading..."}</td>
                <td>{row.summary || "Loading..."}</td>
                <td>
                  {row.previewLink ? (
                    <a href={row.previewLink} target="_blank" rel="noopener noreferrer">
                      Preview
                    </a>
                  ) : (
                    "Pending"
                  )}
                </td>
                <td>
                  {row.status === "Pending" ? (
                    <span>
                      Pending
                      <button
                        className={styles.reloadButton}
                        onClick={() => handleReload(row.recNum)}
                      >
                        &#x21bb; {/* Reload Icon */}
                      </button>
                    </span>
                  ) : (
                    row.status
                  )}
                </td>
              </tr>
            ))
          ) : (
            <tr>
              <td colSpan="6">No data available</td>
            </tr>
          )}
        </tbody>
      </table>
    </div>
  );
};

export default MainContent;



.reloadButton {
  background: none;
  border: none;
  color: blue;
  cursor: pointer;
  margin-left: 8px;
  font-size: 16px;
}

.reloadButton:hover {
  color: darkblue;
}





import React, { useState } from 'react';
import axios from 'axios';
import Sidebar from './Sidebar';
import MainContent from './MainContent';
import './ProductSheetsPage.css';

const ProductSheetsPage = () => {
  const [file, setFile] = useState(null);
  const [message, setMessage] = useState("");
  const [uploading, setUploading] = useState(false);
  const [rows, setRows] = useState([]);

  const handleFileChange = (e) => {
    const selectedFile = e.target.files[0];
    if (selectedFile) {
      setFile(selectedFile);
    }
  };

  const handleUpload = async () => {
    if (!file) {
      setMessage("Please select a file to upload.");
      return;
    }
    setUploading(true);
    try {
      // Request the presigned URL from the backend
      const response = await axios.post("/your-api-endpoint-to-get-presigned-url", {
        payload: { filename: file.name },
      });

      const { presignedUrl, key } = response.data;

      // Use the presigned URL to upload the file
      await axios.put(presignedUrl, file, {
        headers: { "Content-Type": file.type },
      });

      // Add a new row with static RecNum and placeholders
      const newRecNum = rows.length + 1;
      setRows((prevRows) => [
        ...prevRows,
        {
          recNum: newRecNum,
          policyId: "",
          productSheetType: "",
          summary: "",
          previewLink: `https://your-s3-bucket-url.com/${key}`,
          status: "Pending",
        },
      ]);

      setMessage("File uploaded successfully!");
    } catch (error) {
      setMessage("Upload failed: " + error.message);
    } finally {
      setUploading(false);
    }
  };

  const handleReload = async (recNum) => {
    try {
      // Fetch the data for the specific RecNum from the API
      const response = await axios.get(`/your-api-endpoint-to-fetch-data/${recNum}`);
      const data = response.data;

      // Update the row with the fetched data
      setRows((prevRows) =>
        prevRows.map((row) =>
          row.recNum === recNum
            ? {
                ...row,
                policyId: data.policyId,
                productSheetType: data.productSheetType,
                summary: data.summary,
                status: "Completed",
              }
            : row
        )
      );
    } catch (error) {
      setMessage("Failed to fetch data for RecNum: " + recNum);
    }
  };

  return (
    <div className="container">
      <Sidebar onFileChange={handleFileChange} onUpload={handleUpload} uploading={uploading} />
      <MainContent message={message} rows={rows} handleReload={handleReload} />
    </div>
  );
};

export default ProductSheetsPage;
