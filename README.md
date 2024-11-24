import React from 'react';
import styles from './DataTable.module.css';

const DataTable = ({ rows, handleReload }) => {
  return (
    <div className={styles.tableContainer}>
      <table className={styles.table}>
        <thead>
          <tr>
            <th>Rec Num</th>
            <th>Policy ID</th>
            <th>Type</th>
            <th>Summary</th>
            <th>Status</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          {rows.length > 0 ? (
            rows.map((row) => (
              <tr key={row.recNum}>
                <td>{row.recNum}</td>
                <td>{row.policyid || "N/A"}</td>
                <td>{row.type || "N/A"}</td>
                <td>{row.summary || "N/A"}</td>
                <td>{row.status}</td>
                <td>
                  <button
                    onClick={() => handleReload(row.recNum)}
                    className={styles.reloadButton}
                  >
                    Reload
                  </button>
                </td>
              </tr>
            ))
          ) : (
            <tr>
              <td colSpan="6" className={styles.noData}>
                No data available. Upload a file to get started.
              </td>
            </tr>
          )}
        </tbody>
      </table>
    </div>
  );
};

export default DataTable;




import React, { useState } from 'react';
import axios from 'axios';
import Sidebar from './Sidebar';
import MainContent from './MainContent';
import DataTable from './DataTable'; // Import the new DataTable component
import styles from './ProductSheetsPage.module.css';

const ProductSheetsPage = () => {
  const [file, setFile] = useState(null);
  const [message, setMessage] = useState("");
  const [uploading, setUploading] = useState(false);
  const [rows, setRows] = useState([]);
  const [isUploaded, setIsUploaded] = useState(false);
  const [showMainContent, setShowMainContent] = useState(false);

  const handleFileChange = (e) => {
    const selectedFile = e.target.files[0];
    if (selectedFile) {
      setFile(selectedFile);
      setMessage("");
    }
  };

  const handleUpload = async () => {
    if (!file) {
      setMessage("Please select a file to upload.");
      return;
    }
    setUploading(true);
    try {
      const response = await axios.post("dummy1", {
        payload: { filename: file.name },
      });

      const { presignedUrl, key, recNum } = response.data;

      await axios.put(presignedUrl, file, {
        headers: { "Content-Type": file.type },
      });

      const sqsPayload = {
        claimid: recNum,
        s3filename: key,
        tasktype: "SEND_TO_QUEUE",
      };

      await axios.post("dummy2", sqsPayload, {
        headers: { "Content-Type": "application/json" },
      });

      setRows((prevRows) => [
        ...prevRows,
        {
          recNum,
          policyid: "",
          type: "",
          summary: "",
          previewLink: presignedUrl,
          status: "Pending",
        },
      ]);

      setMessage("File uploaded successfully!");
      setIsUploaded(true);
    } catch (error) {
      setMessage("Upload failed: " + error.message);
    } finally {
      setUploading(false);
    }
  };

  const handleReload = async (recNum) => {
    try {
      const payload = {
        tasktype: "FETCH_SINGLE_CLAIM",
        claimid: recNum,
      };

      const response = await axios.post(`dummy3`, payload, {
        headers: { "Content-Type": "application/json" },
      });

      const data = response.data;

      setRows((prevRows) =>
        prevRows.map((row) =>
          row.recNum === recNum
            ? {
                ...row,
                policyid: data.policyid,
                type: data.type,
                summary: data.summary,
                status: "Completed️✅",
              }
            : row
        )
      );
    } catch (error) {
      setMessage("Failed to fetch data for RecNum: " + recNum);
    }
  };

  return (
    <div className={styles.container}>
      {!showMainContent ? (
        <>
          <button
            className={styles.toggleButton}
            onClick={() => setShowMainContent(true)}
          >
            Switch to Sidebar and MainContent
          </button>
          <DataTable rows={rows} handleReload={handleReload} /> {/* Use DataTable */}
        </>
      ) : (
        <>
          <button
            className={styles.toggleButton}
            onClick={() => setShowMainContent(false)}
          >
            Back to Table View
          </button>
          <Sidebar
            onFileChange={handleFileChange}
            onUpload={handleUpload}
            uploading={uploading}
          />
          {isUploaded ? (
            <MainContent
              message={message}
              rows={rows}
              handleReload={handleReload}
            />
          ) : (
            <p className={styles.infoMessage}>
              Please upload a document to view the data.
            </p>
          )}
        </>
      )}
    </div>
  );
};

export default ProductSheetsPage;



.tableContainer {
  margin-top: 20px;
  overflow-x: auto;
}

.table {
  width: 100%;
  border-collapse: collapse;
}

.table th,
.table td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

.table th {
  background-color: #f4f4f4;
}

.noData {
  text-align: center;
  color: #777;
}

.reloadButton {
  padding: 5px 10px;
  background-color: #28a745;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.reloadButton:hover {
  background-color: #218838;
}
