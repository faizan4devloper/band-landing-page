import React, { useState } from "react";
import './ProductSheetsPage.css'; // Custom CSS for styling

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
      const response = await fetch("https://<your-api-gateway-url>", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(payload),
      });

      const data = await response.json();
      const { url } = data;

      // Upload the file to S3 using the pre-signed URL
      await fetch(url, {
        method: "PUT",
        body: file,
        headers: {
          "Content-Type": file.type,
        },
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
    <div className="container">
      <h1>Manage Product Sheets</h1>
      <div className="upload-section">
        <input type="file" accept="application/pdf" onChange={handleFileChange} />
        <button onClick={handleUpload}>Upload Product Sheet</button>
      </div>
      {message && <p className="message">{message}</p>}

      {/* Table to display uploaded product sheets */}
      <div className="table-container">
        <table>
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
