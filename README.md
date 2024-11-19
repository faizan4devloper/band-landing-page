import React, { useState } from "react";
import axios from "axios";
import styles from './ProductSheetsPage.module.css';

const ProductSheetsPage = () => {
  const [file, setFile] = useState(null); // State to hold the file
  const [uploadStatus, setUploadStatus] = useState(""); // Status for file upload
  const [fileDetails, setFileDetails] = useState([]); // Array to store file details for the table

  // Handle file selection
  const handleFileChange = (e) => {
    setFile(e.target.files[0]);
  };

  // Function to upload the file to S3 using presigned URL
  const handleUpload = async () => {
    if (!file) {
      setUploadStatus("No file selected.");
      return;
    }

    try {
      // 1. Step - Request presigned URL from backend
      const response = await axios.post('https://your-api-endpoint.com/presign-url', {
        payload: {
          filename: file.name,
        },
      });

      const { presignedUrl, key } = response.data;

      // 2. Step - Use the presigned URL to upload the file to S3
      const uploadResponse = await axios.put(presignedUrl, file, {
        headers: {
          "Content-Type": file.type,
        },
      });

      setUploadStatus("File uploaded successfully!");

      // 3. Step - Add the file details to the table
      const newFileDetails = {
        policyId: key.split('/')[2],  // Extract Policy ID from S3 key
        productSheetType: file.type,
        summary: file.name,
        previewLink: `https://your-s3-bucket-url/${key}`,
      };

      setFileDetails([...fileDetails, newFileDetails]);
    } catch (error) {
      console.error("Upload failed:", error);
      setUploadStatus("Upload failed.");
    }
  };

  return (
    <div className={styles.container}>
      {/* Left Sidebar */}
      <div className={styles.sidebar}>
        <input type="file" onChange={handleFileChange} />
        <button className={styles.uploadButton} onClick={handleUpload}>
          Upload & Process
        </button>
        {uploadStatus && <p>{uploadStatus}</p>}
      </div>

      {/* Main Content - Table Display */}
      <div className={styles.mainContent}>
        <h2>Uploaded Product Sheets</h2>
        <table className={styles.table}>
          <thead>
            <tr>
              <th>Policy ID</th>
              <th>Product Sheet Type</th>
              <th>Summary</th>
              <th>Preview Link</th>
            </tr>
          </thead>
          <tbody>
            {fileDetails.map((file, index) => (
              <tr key={index}>
                <td>{file.policyId}</td>
                <td>{file.productSheetType}</td>
                <td>{file.summary}</td>
                <td>
                  <a href={file.previewLink} target="_blank" rel="noopener noreferrer">
                    View PDF
                  </a>
                </td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>
    </div>
  );
};

export default ProductSheetsPage;



.container {
  display: flex;
  padding: 20px;
}

.sidebar {
  width: 300px;
  margin-right: 20px;
}

.mainContent {
  flex-grow: 1;
}

.uploadButton {
  background-color: #4CAF50;
  color: white;
  padding: 10px 20px;
  border: none;
  cursor: pointer;
  margin-top: 10px;
}

.uploadButton:hover {
  background-color: #45a049;
}

.table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

.table th, .table td {
  padding: 10px;
  border: 1px solid #ddd;
  text-align: left;
}

.table th {
  background-color: #f2f2f2;
}
