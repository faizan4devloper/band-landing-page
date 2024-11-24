import React, { useState } from 'react';
import axios from 'axios';
import Sidebar from './Sidebar';
import MainContent from './MainContent';
import DataTable from './DataTable';
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
      const response = await axios.post("https://41aw3s5s3k.execute-api.us-east-1.amazonaws.com/dev/", {
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

      const sqsResponse = await axios.post("https://e21wxu9skj.execute-api.us-east-1.amazonaws.com/dev/querequest", sqsPayload, {
        headers: { "Content-Type": "application/json" },
      });

      console.log("SQS API Response:", sqsResponse.data);

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

      const response = await axios.post(`dummy`, payload, {
        headers: { "Content-Type": "application/json" },
      });

      console.log("Reload Data:", recNum, response.data);
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
        <div className={styles.mainContentWrapper}>
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
        </div>
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


.container {
  padding: 5px 20px;
  display: grid;
  grid-template-columns: 1fr 3fr; /* 1 part for Sidebar, 3 parts for MainContent */
  gap: 20px; /* Adds space between Sidebar and MainContent */
}

.infoMessage {
  text-align: center;
  margin-top: 180px;
  font-size: 1.2rem;
  color: #555;
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
  width: fit-content; /* Ensure button does not take unnecessary space */
  margin-bottom: 20px; /* To provide some space between button and table */
}

.newClaimButton:hover {
  background-color: #0056b3;
}

/* Adjusting the layout of the DataTable or MainContent when displayed */
.mainContentWrapper {
  display: flex;
  flex-direction: column; /* Stack elements vertically */
  gap: 20px; /* Space between content elements */
}
