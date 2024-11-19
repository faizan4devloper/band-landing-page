import React, { useState } from "react";
import styles from './ProductSheetsPage.module.css'; // Assuming you're using CSS Modules for styling.
import { Table, TableBody, TableCell, TableContainer, TableHead, TableRow, Paper, Button } from '@mui/material';
import axios from "axios";

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

      // Fetch pre-signed URL from API Gateway
      const response = await axios.post("https://<your-api-gateway-url>", payload, {
        headers: { "Content-Type": "application/json" },
      });

      const { url } = response.data;

      // Upload the file to S3 using the pre-signed URL
      await axios.put(url, file, {
        headers: { "Content-Type": file.type },
      });

      // Update the list of uploaded product sheets
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
        <Button variant="contained" color="primary" onClick={handleUpload} style={{ marginLeft: "10px" }}>
          Upload Product Sheet
        </Button>
      </div>
      {message && <p className={styles.message}>{message}</p>}

      {/* Table to display uploaded product sheets */}
      <TableContainer component={Paper} style={{ marginTop: "20px" }}>
        <Table>
          <TableHead>
            <TableRow>
              <TableCell><b>File Name</b></TableCell>
              <TableCell><b>File Type</b></TableCell>
              <TableCell><b>Status</b></TableCell>
            </TableRow>
          </TableHead>
          <TableBody>
            {productSheets.map((sheet, index) => (
              <TableRow key={index}>
                <TableCell>{sheet.fileName}</TableCell>
                <TableCell>{sheet.fileType}</TableCell>
                <TableCell>{sheet.status}</TableCell>
              </TableRow>
            ))}
          </TableBody>
        </Table>
      </TableContainer>
    </div>
  );
};

export default ProductSheetsPage;


.container {
  padding: 20px;
  font-family: Arial, sans-serif;
}

.uploadSection {
  margin-bottom: 20px;
  display: flex;
  align-items: center;
}

.message {
  margin-top: 10px;
  color: green;
  font-weight: bold;
}

table {
  margin-top: 20px;
}

table th, table td {
  text-align: left;
  padding: 10px;
}

table th {
  background-color: #f4f4f4;
}

table tr:nth-child(even) {
  background-color: #f9f9f9;
}
