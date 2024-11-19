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

  // Function to handle file selection
  const handleFileChange = (e) => {
    const selectedFile = e.target.files[0];
    if (selectedFile) {
      setFile(selectedFile);
    }
  };

  // Function to handle file upload
  const handleUpload = async () => {
    if (!file) {
      setMessage("Please select a file to upload.");
      return;
    }
    setUploading(true);
    try {
      // Request the presigned URL from the backend
      const response = await axios.post("dummy", {
        filename: file.name,
      });

      const { presignedUrl, key } = response.data;

      // Upload the file to S3 using the presigned URL
      await axios.put(presignedUrl, file, {
        headers: { "Content-Type": file.type },
      });

      // Add a new row with static RecNum and placeholders
      const newRecNum = rows.length + 1;
      setRows((prevRows) => [
        ...prevRows,
        {
          recNum: newRecNum,
          policyId: "",
          productSheetType: "",
          summary: "",
          previewLink: `https://your-s3-bucket-url.com/${key}`,
          status: "Pending",
        },
      ]);

      setMessage("File uploaded successfully!");
    } catch (error) {
      setMessage("Upload failed: " + error.message);
      console.error("Upload Error:", error);
    } finally {
      setUploading(false);
    }
  };

  // Function to reload row data using RecNum
  const handleReload = async (recNum) => {
    try {
      const payload = {
        recNum,
        filename: file?.name || "unknown-file", // Ensure filename is included in the payload
      };

      const headers = {
        "Content-Type": "application/json",
      };

      // Fetch the data for the specific RecNum from the API
      const response = await axios.post("dummy1", payload, { headers });

      const data = response.data;
      console.log("Reload Data:", data);

      // Update the row with the fetched data
      setRows((prevRows) =>
        prevRows.map((row) =>
          row.recNum === recNum
            ? {
                ...row,
                policyId: data.policyId || "N/A",
                productSheetType: data.productSheetType || "N/A",
                summary: data.summary || "N/A",
                status: "Completed",
              }
            : row
        )
      );

      setMessage(`Data for RecNum ${recNum} reloaded successfully.`);
    } catch (error) {
      setMessage(`Failed to fetch data for RecNum: ${recNum}`);
      console.error("Reload Error:", error);
    }
  };

  return (
    <div className={styles.container}>
      <Sidebar
        onFileChange={handleFileChange}
        onUpload={handleUpload}
        uploading={uploading}
      />
      <MainContent
        message={message}
        rows={rows}
        handleReload={handleReload}
      />
    </div>
  );
};

export default ProductSheetsPage;
