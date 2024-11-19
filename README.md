import React from 'react';
import styles from './Sidebar.module.css'; // Custom CSS for Sidebar

const Sidebar = ({ onFileChange, onUpload, uploading }) => {
  return (
    <div className={styles.sidebar}>
      <h3>Upload Product Sheet</h3>
      <input type="file" onChange={onFileChange} />
      <button
        className={styles.uploadButton}
        onClick={onUpload}
        disabled={uploading}
      >
        {uploading ? "Uploading..." : "Upload"}
      </button>
    </div>
  );
};

export default Sidebar;



.sidebar {
  width: 250px;
  padding: 20px;
  background-color: #f4f4f4;
  border-radius: 8px;
}

.uploadButton {
  margin-top: 10px;
  padding: 10px 20px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.uploadButton:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}




import React from 'react';
import styles from './MainContent.module.css'; // Custom CSS for MainContent

const MainContent = ({ message, uploading, rows }) => {
  return (
    <div className={styles.mainContent}>
      <h2>Uploaded Product Sheets</h2>
      {message && <p>{message}</p>}
      
      {/* Table to display uploaded data */}
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
          {rows && rows.length > 0 ? (
            rows.map((row, index) => (
              <tr key={index}>
                <td>{row.policyId}</td>
                <td>{row.productSheetType}</td>
                <td>{row.summary}</td>
                <td>
                  <a href={row.previewLink} target="_blank" rel="noopener noreferrer">
                    Preview
                  </a>
                </td>
              </tr>
            ))
          ) : (
            <tr>
              <td colSpan="4">No data available</td>
            </tr>
          )}
        </tbody>
      </table>
    </div>
  );
};

export default MainContent;



.mainContent {
  margin-left: 20px;
  flex-grow: 1;
  padding: 20px;
}

.table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

.table th, .table td {
  padding: 12px;
  border: 1px solid #ddd;
  text-align: left;
}

.table th {
  background-color: #f2f2f2;
}



import React, { useState } from 'react';
import axios from 'axios';
import Sidebar from './Sidebar';  // Import Sidebar
import MainContent from './MainContent';  // Import MainContent
import './ProductSheetsPage.css';  // Optional global styles

const ProductSheetsPage = () => {
  const [file, setFile] = useState(null);
  const [message, setMessage] = useState("");
  const [uploading, setUploading] = useState(false);
  const [rows, setRows] = useState([]); // To store uploaded rows

  // Function to handle file input change
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
      const response = await axios.post("/your-api-endpoint-to-get-presigned-url", {
        payload: {
          filename: file.name,
        },
      });

      const { presignedUrl, key } = response.data;

      // Now use the presigned URL to upload the file directly to S3
      const uploadResponse = await axios.put(presignedUrl, file, {
        headers: {
          "Content-Type": file.type,
        },
      });

      // Handle the successful upload
      if (uploadResponse.status === 200) {
        setMessage("File uploaded successfully!");
        
        // Add the uploaded file details to the rows
        setRows([...rows, {
          policyId: '12345',
          productSheetType: 'Claim Form',
          summary: 'Test summary',
          previewLink: `https://your-s3-bucket-url.com/${key}`,
        }]);
      } else {
        setMessage("Error uploading the file.");
      }
    } catch (error) {
      setMessage("Upload failed: " + error.message);
    } finally {
      setUploading(false);
    }
  };

  return (
    <div className="container">
      <Sidebar onFileChange={handleFileChange} onUpload={handleUpload} uploading={uploading} />
      <MainContent message={message} uploading={uploading} rows={rows} />
    </div>
  );
};

export default ProductSheetsPage;



.container {
  display: flex;
  padding: 20px;
}

.container > div {
  margin-right: 20px;
}
