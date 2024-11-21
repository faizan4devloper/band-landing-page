import React, { useState } from 'react';
import axios from 'axios';
import Sidebar from './Sidebar';
import MainContent from './MainContent';
import styles from './ProductSheetsPage.module.css';

const ProductSheetsPage = () => {
  const [file, setFile] = useState(null);
  const [message, setMessage] = useState("");
  const [uploading, setUploading] = useState(false);
  const [rows, setRows] = useState([]);

  const handleFileChange = (e) => {
    const selectedFile = e.target.files[0];
    if (selectedFile) {
      setFile(selectedFile);
    }
  };

  const handleUpload = async () => {
    if (!file) {
      setMessage("Please select a file to upload.");
      return;
    }
    setUploading(true);
    try {
      // Request the presigned URL from the backend
      const response = await axios.post("dummy", {
        payload: { filename: file.name },
      });
      
      console.log(response.data)

      const { presignedUrl, key } = response.data;

      // Use the presigned URL to upload the file
      await axios.put(presignedUrl, file, {
        headers: { "Content-Type": file.type },
      });

      // Add a new row with static RecNum and placeholders
      const newRecNum = rows.length + 1;
      setRows((prevRows) => [
        ...prevRows,
        {
          recNum: newRecNum,
          policyid: "",
          type: "",
          summary: "",
          previewLink: `https://your-s3-bucket-url.com/${key}`,
          status: "Pending",
        },
      ]);
  
      setMessage("File uploaded successfully!");
    } catch (error) {
      setMessage("Upload failed: " + error.message);
    } finally {
      setUploading(false);
    }
  };

 
const handleReload = async (recNum) => {
  try {
    // Define the payload with the required properties
    const payload = {
      recNum, // Add the RecNum value dynamically
      fileType: file ? file.type : "application/json", // Example: dynamic or default type
      filename: file ? file.name : "example.txt", // Example: dynamic or default filename
    };

    // Make the API call with the payload
    const response = await axios.post(`dummy`, payload, {
      headers: { "Content-Type": "application/json" },
    });
    


    console.log("Reload Data:", recNum, response.data);
    const data = response.data;

    // Update the row with the fetched data
    setRows((prevRows) =>
      prevRows.map((row) =>
        row.recNum === recNum
          ? {
              ...row,
              policyid: data.policyid,
              type: data.type,
              summary: data.summary,
              status: "Completed✔️",
            }
          : row
      )
    );
  } catch (error) {
    setMessage("Failed to fetch data for RecNum: " + recNum);
    console.error("Reload Error:", error);
  }
};

  return (
    <div className={styles.container}>
      <Sidebar onFileChange={handleFileChange} onUpload={handleUpload} uploading={uploading} />
      <MainContent message={message} rows={rows} handleReload={handleReload} />
    </div>
  );
};

export default ProductSheetsPage;
