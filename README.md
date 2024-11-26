.container {
  display: flex;
  height: 100vh; /* Full viewport height */
  width: 100%; /* Full width */
  overflow: hidden; /* Prevent overflow issues */
}

.sidebar {
  flex: 0 0 300px; /* Fixed width for sidebar (can be adjusted) */
  height: 100%;
  background-color: #f4f4f4; /* Light gray for better visual separation */
  border-right: 1px solid #ddd;
  padding: 20px;
  box-sizing: border-box; /* Include padding in width/height calculations */
}

.mainContent {
  flex: 1; /* Take up remaining space */
  height: 100%;
  background-color: #fff; /* White background */
  overflow-y: auto; /* Allow vertical scrolling for large content */
  padding: 20px;
  box-sizing: border-box;
}

.infoMessage {
  font-size: 1rem;
  color: #555;
  text-align: center;
  margin: auto;
}



import React, { useState } from "react";
import axios from "axios";

import Sidebar from "./Sidebar";
import MainContent from "./MainContent";
import styles from "./NewClaimPage.module.css";

const NewClaimPage = () => {
  const [file, setFile] = useState(null);
  const [message, setMessage] = useState("");
  const [uploading, setUploading] = useState(false);
  const [rows, setRows] = useState([]);
  const [isUploaded, setIsUploaded] = useState(false);

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
      const response = await axios.post(
        "https://41aw3s5s3k.execute-api.us-east-1.amazonaws.com/dev/",
        {
          payload: { filename: sanitizedFileName, filetype: "CL" },
        }
      );

      const { presignedUrl, key, recNum } = response.data;

      await axios.put(presignedUrl, file, {
        headers: { "Content-Type": file.type },
      });

      const sqsPayload = {
        claimid: recNum,
        s3filename: key,
        tasktype: "SEND_TO_QUEUE",
      };

      await axios.post(
        "https://e21wxu9skj.execute-api.us-east-1.amazonaws.com/dev/querequest",
        sqsPayload,
        { headers: { "Content-Type": "application/json" } }
      );

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
      setMessage("Upload failed: " + error.message);
    } finally {
      setUploading(false);
    }
  };

  return (
    <div className={styles.container}>
      <div className={styles.sidebar}>
        <Sidebar
          onFileChange={handleFileChange}
          onUpload={handleUpload}
          uploading={uploading}
        />
      </div>
      <div className={styles.mainContent}>
        {isUploaded ? (
          <MainContent message={message} rows={rows} setRows={setRows} />
        ) : (
          <p className={styles.infoMessage}>
            Please upload a document to view the data.
          </p>
        )}
      </div>
    </div>
  );
};

export default NewClaimPage;
