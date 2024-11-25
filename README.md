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
      if (selectedFile.type === "application/pdf") {
        setFile(selectedFile);
        setMessage("");
      } else {
        setMessage("Only PDF files are allowed. Please select a valid file.");
      }
    }
  };

  const handleUpload = async () => {
    if (!file) {
      setMessage("Please select a file to upload.");
      return;
    }

    setUploading(true);

    try {
      const sanitizedFileName = file.name.replace(/\s+/g, '_');
      console.log("Uploading file:", sanitizedFileName);

      // Step 1: Request presigned URL
      const response = await axios.post("dummy", {
        payload: { filename: sanitizedFileName },
      });

      console.log("Presigned URL Response:", response.data);

      const { presignedUrl, key, recNum } = response.data;

      // Step 2: Upload the file to S3 using the presigned URL
      await axios.put(presignedUrl, file, {
        headers: { "Content-Type": file.type || "application/pdf" },
      });

      console.log("File uploaded successfully to S3.");

      // Step 3: Notify backend for further processing
      const sqsPayload = {
        claimid: recNum, // Ensure claimid uses recNum
        psfilename: key, // Updated to use `ps` instead of `cl` and `ci`
        actionn: "transform",
        tasktype: "SEND_TO_PS_QUEUE",
      };

      const sqsResponse = await axios.post("dummy", sqsPayload, {
        headers: { "Content-Type": "application/json" },
      });

      console.log("SQS API Response:", sqsResponse.data);

      // Update rows with the new record
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

      setIsUploaded(true);
    } catch (error) {
      console.error("Upload failed:", error);
      setMessage("Upload failed: " + (error.response?.data?.message || error.message));
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

      const data = response.data;
      console.log("Reload Data:", data);

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
      console.error("Reload failed:", error);
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
