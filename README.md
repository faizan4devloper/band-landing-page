import React, { useState } from "react";
import axios from "axios";
import Sidebar from "./Sidebar";
import MainContent from "./MainContent";
import DataTable from "./DataTable";
import styles from "./ProductSheetsPage.module.css";

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
      const sanitizedFileName = file.name.replace(/\s+/g, "_");

      const response = await axios.post("dummy", {
        payload: { filename: sanitizedFileName },
      });

      console.log("API Response 1:", response.data);

      const { presignedUrl, key, recNum } = response.data;

      await axios.put(presignedUrl, file, {
        headers: { "Content-Type": file.type },
      });

      const sqsPayload = {
        claimid: recNum,
        s3filename: key,
        actionn: "transform",
        tasktype: "SEND_TO_PS_QUEUE",
      };

      const sqsResponse = await axios.post("dummy", sqsPayload, {
        headers: { "Content-Type": "application/json" },
      });

      console.log("SQS API Response:", sqsResponse.data);

      setRows((prevRows) => [
        ...prevRows,
        {
          recNum,
          policy_id: "",
          prod_sheet_type: "",
          summary: "",
          previewLink: presignedUrl,
          status: "Pending",
        },
      ]);

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

      const response = await axios.post("dummy", payload, {
        headers: { "Content-Type": "application/json" },
      });

      console.log("API Response:", response);

      if (Array.isArray(response.data) && response.data.length > 0) {
        const singleClaimData = response.data[0];
        console.log("Single Claim Data:", singleClaimData);

        const {
          policy_id = "N/A",
          file_name = "N/A",
          status = "Unknown",
          summary = "No summary available",
          prod_sheet_type = "Unknown",
          rec_number = recNum,
        } = singleClaimData;

        console.log("Policy ID:", policy_id);
        console.log("File Name:", file_name);
        console.log("Status:", status);
        console.log("Summary:", summary);
        console.log("Product Sheet Type:", prod_sheet_type);
        console.log("Record Number:", rec_number);

        setRows((prevRows) =>
          prevRows.map((row) =>
            row.recNum === recNum
              ? {
                  ...row,
                  policy_id,
                  prod_sheet_type,
                  summary,
                  status,
                  previewLink: row.previewLink,
                }
              : row
          )
        );
      } else {
        console.error("Unexpected response format:", response.data);
        setMessage("Failed to fetch claim details.");
      }
    } catch (error) {
      console.error("Error fetching claim data:", error.message);
      setMessage("Failed to fetch data for RecNum: " + recNum);
    }
  };

  return (
    <div className={styles.container}>
      {!showMainContent ? (
        <>
          <div className={styles.header}>
            <button
              className={styles.newClaimButton}
              onClick={() => setShowMainContent(true)}
            >
              New Claim Processing
            </button>
          </div>
          <DataTable rows={rows} handleReload={handleReload} />
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
