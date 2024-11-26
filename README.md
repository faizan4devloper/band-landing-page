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
        "dummy",
        {
          payload: { filename: sanitizedFileName, filetype: "CL" },
        }
      );
      
      console.log("Presign url", response.data)

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
