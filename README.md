import React, { useState } from "react";
import axios from "axios";
import styles from "./ProductSheetsPage.module.css"; // Importing CSS module

const ProductSheetsPage = () => {
  const [file, setFile] = useState(null);
  const [productSheets, setProductSheets] = useState([]);
  const [message, setMessage] = useState("");

  const handleFileChange = (event) => {
    setFile(event.target.files[0]);
  };

  const handleUpload = async () => {
    if (!file) {
      setMessage("Please select a file to upload.");
      return;
    }

    try {
      const payload = {
        payload: {
          filename: file.name,
        },
      };

      // Request pre-signed URL from API Gateway
      const { data } = await axios.post("https://<your-api-gateway-url>", payload, {
        headers: { "Content-Type": "application/json" },
      });

      const { url } = data;

      // Upload the file to S3 using the pre-signed URL
      await axios.put(url, file, {
        headers: { "Content-Type": file.type },
      });

      // Update the table with uploaded file data
      setProductSheets((prevSheets) => [
        ...prevSheets,
        {
          fileName: file.name,
          fileType: file.type,
          status: "Uploaded Successfully",
        },
      ]);

      setMessage("File uploaded successfully!");
      setFile(null); // Clear the file input
    } catch (error) {
      console.error("Upload failed:", error);
      setMessage("Failed to upload the file.");
      setProductSheets((prevSheets) => [
        ...prevSheets,
        {
          fileName: file.name,
          fileType: file.type,
          status: "Upload Failed",
        },
      ]);
    }
  };

  return (
    <div className={styles.container}>
      <h1>Manage Product Sheets</h1>
      <div className={styles.uploadSection}>
        <input type="file" accept="application/pdf" onChange={handleFileChange} />
        <button className={styles.uploadButton} onClick={handleUpload}>
          Upload Product Sheet
        </button>
      </div>
      {message && <p className={styles.message}>{message}</p>}

      <div className={styles.tableContainer}>
        <table className={styles.table}>
          <thead>
            <tr>
              <th>File Name</th>
              <th>File Type</th>
              <th>Status</th>
            </tr>
          </thead>
          <tbody>
            {productSheets.map((sheet, index) => (
              <tr key={index}>
                <td>{sheet.fileName}</td>
                <td>{sheet.fileType}</td>
                <td>{sheet.status}</td>
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
  padding: 20px;
  font-family: Arial, sans-serif;
}

h1 {
  margin-bottom: 20px;
}

.uploadSection {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 20px;
}

.uploadButton {
  padding: 10px 15px;
  border: none;
  background-color: #007bff;
  color: white;
  cursor: pointer;
  border-radius: 5px;
  transition: background-color 0.3s;
}

.uploadButton:hover {
  background-color: #0056b3;
}

.message {
  color: green;
  font-weight: bold;
  margin-top: 10px;
}

.tableContainer {
  margin-top: 20px;
}

.table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  padding: 10px;
  text-align: left;
  border: 1px solid #ddd;
}

th {
  background-color: #f4f4f4;
}

tr:nth-child(even) {
  background-color: #f9f9f9;
}

tr:hover {
  background-color: #f1f1f1;
}
