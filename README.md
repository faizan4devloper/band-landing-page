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
          {/* New Claim Processing Button */}
          <div className={styles.header}>
            <button
              className={styles.newClaimButton}
              onClick={() => setShowMainContent(true)}
            >
              New Claim Processing
            </button>
          </div>
          <DataTable rows={rows} handleReload={handleReload} /> {/* Use DataTable */}
        </>
      ) : (
        <>
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



.header {
  display: flex;
  justify-content: flex-start;
  margin-bottom: 20px;
}

.newClaimButton {
  background-color: #007bff;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.newClaimButton:hover {
  background-color: #0056b3;
}
