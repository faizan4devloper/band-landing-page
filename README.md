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
    } finally {
      setUploading(false);
    }
  };

  const handleReload = async (recNum) => {
    try {
      // Fetch the data for the specific RecNum from the API
    //   const response = await axios.post(`https://41aw3s5s3k.execute-api.us-east-1.amazonaws.com/dev/fileprocessing%22${recNum}`);
     const response = await axios.post(`dummy1${recNum}`,{
                 headers: { "Content-Type": file.type },

      });
      
      console.log(response)
      const data = response.data;
console.log("Reload Data:",data);
      // Update the row with the fetched data
      setRows((prevRows) =>
        prevRows.map((row) =>
          row.recNum === recNum
            ? {
                ...row,
                policyId: data.policyId,
                productSheetType: data.productSheetType,
                summary: data.summary,
                status: "Completed",
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
      <Sidebar onFileChange={handleFileChange} onUpload={handleUpload} uploading={uploading} />
      <MainContent message={message} rows={rows} handleReload={handleReload} />
    </div>
  );
};

export default ProductSheetsPage;
