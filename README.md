import React, { useState } from "react";
import axios from "axios";
import Sidebar from "./Sidebar";
import MainContent from "./MainContent";
import styles from "./NewClaimPage.module.css";

const NewClaimPage = () => {
  const [file, setFile] = useState(null);
  const [previewUrl, setPreviewUrl] = useState(""); // Store local preview URL
  const [message, setMessage] = useState("");
  const [uploading, setUploading] = useState(false);
  const [rows, setRows] = useState([]);
  const [isUploaded, setIsUploaded] = useState(false);

  const handleFileChange = (e) => {
    const selectedFile = e.target.files[0];
    if (selectedFile) {
      setFile(selectedFile);
      setPreviewUrl(URL.createObjectURL(selectedFile)); // Generate preview URL
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
        payload: { filename: sanitizedFileName, filetype: "CL" },
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
          <MainContent 
            message={message} 
            rows={rows} 
            setRows={setRows} 
            staticPreviewUrl={previewUrl} // Pass the static preview URL
          />
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




import React, { useState } from "react";
import axios from "axios";
import Modal from "react-modal";
import styles from "./MainContent.module.css";

const MainContent = ({ message, rows, setRows, staticPreviewUrl }) => {
  const [loading, setLoading] = useState(false);

  return (
    <div className={styles.mainContent}>
      {/* Left: Document Preview */}
      <div className={styles.previewSection}>
        <h3>Document Preview</h3>
        {staticPreviewUrl ? (
          <iframe
            src={staticPreviewUrl}
            title="Static Document Preview"
            className={styles.documentPreviewIframe}
          ></iframe>
        ) : (
          <p>No document available for preview.</p>
        )}
      </div>

      {/* Right: Extracted Content */}
      <div className={styles.extractContentSection}>
        <h3>Extracted Content</h3>
        {loading ? <p>Loading...</p> : <p>{message}</p>}
      </div>
    </div>
  );
};

export default MainContent;
