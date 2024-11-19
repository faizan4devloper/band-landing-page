import React, { useState } from "react";
import axios from "axios";
import styles from './ProductSheetsPage.module.css'; // Assuming you have your custom CSS

const ProductSheetsPage = () => {
  const [file, setFile] = useState(null);
  const [message, setMessage] = useState("");
  const [uploading, setUploading] = useState(false);

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
    <div className={styles.container}>
      <div className={styles.sidebar}>
        <h3>Upload Product Sheet</h3>
        <input type="file" onChange={handleFileChange} />
        <button
          className={styles.uploadButton}
          onClick={handleUpload}
          disabled={uploading}
        >
          {uploading ? "Uploading..." : "Upload"}
        </button>
      </div>

      <div className={styles.mainContent}>
        <h2>Uploaded Product Sheets</h2>
        {message && <p>{message}</p>}
        {/* Table to display data */}
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
            {/* Here you can dynamically generate rows from data */}
            <tr>
              <td>12345</td>
              <td>Claim Form</td>
              <td>Test summary</td>
              <td>
                <a href="https://your-s3-bucket-url.com/file.pdf" target="_blank" rel="noopener noreferrer">
                  Preview
                </a>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  );
};

export default ProductSheetsPage;
