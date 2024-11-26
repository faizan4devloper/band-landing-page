import React, { useState } from 'react';
import Sidebar from './Sidebar'; // Import Sidebar component
import MainContent from './MainContent'; // Import MainContent component
import styles from './NewClaimPage.module.css';

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
      const sanitizedFileName = file.name.replace(/\s+/g, '_');
      const response = await axios.post("dummy", {
        payload: { filename: sanitizedFileName, filetype: "PS" },
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
      <Sidebar
        onFileChange={handleFileChange}
        onUpload={handleUpload}
        uploading={uploading}
      />
      {isUploaded ? (
        <MainContent message={message} rows={rows} setRows={setRows} />
      ) : (
        <p className={styles.infoMessage}>
          Please upload a document to view the data.
        </p>
      )}
    </div>
  );
};

export default NewClaimPage;
