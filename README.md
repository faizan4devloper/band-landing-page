import React from "react";
import styles from "./Sidebar.module.css";

const Sidebar = ({ onFileChange, onUpload, uploading, onPolicyChange }) => {
  const handlePolicySelect = (e) => {
    onPolicyChange(e.target.value);
  };

  return (
    <div className={styles.sidebar}>
      <h2>Upload Document</h2>
      <input type="file" onChange={onFileChange} />
      <button onClick={onUpload} disabled={uploading}>
        {uploading ? "Uploading..." : "Upload"}
      </button>

      {/* Dropdown for policy selection */}
      <div className={styles.dropdown}>
        <label htmlFor="policy">Select Policy:</label>
        <select id="policy" onChange={handlePolicySelect}>
          <option value="">--Select--</option>
          <option value="ps12">PS12</option>
          <option value="ps21">PS21</option>
        </select>
      </div>
    </div>
  );
};

export default Sidebar;




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
  const [previewUrl, setPreviewUrl] = useState(""); // State for static preview URL
  const [selectedPolicy, setSelectedPolicy] = useState(""); // State for policy selection

  const handleFileChange = (e) => {
    const selectedFile = e.target.files[0];
    if (selectedFile) {
      setFile(selectedFile);
      setPreviewUrl(URL.createObjectURL(selectedFile)); // Generate preview URL
      setMessage("");
    }
  };

  const handlePolicyChange = (policy) => {
    setSelectedPolicy(policy);
    console.log("Selected Policy:", policy);
  };

  const handleUpload = async () => {
    if (!file) {
      setMessage("Please select a file to upload.");
      return;
    }
    if (!selectedPolicy) {
      setMessage("Please select a policy.");
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
      console.log("Presign url", response.data);

      const { presignedUrl, key, recNum } = response.data;

      await axios.put(presignedUrl, file, {
        headers: { "Content-Type": file.type },
      });

      const sqsPayload = {
        claimid: recNum,
        s3filename: key,
        tasktype: "SEND_TO_QUEUE",
        policy: selectedPolicy, // Include the selected policy
      };

      const sqsResponse = await axios.post(
        "dummy",
        sqsPayload,
        { headers: { "Content-Type": "application/json" } }
      );

      console.log("SQS API Response:", sqsResponse.data);

      setRows((prevRows) => [
        ...prevRows,
        {
          recNum,
          policyid: selectedPolicy, // Set policy in rows
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
          onPolicyChange={handlePolicyChange}
        />
      </div>
      <div className={styles.mainContent}>
        {isUploaded ? (
          <MainContent 
            message={message} 
            rows={rows} 
            clid={rows[0].recNum}
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
