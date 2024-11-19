import React, { useState, useCallback } from "react";
import { useDropzone } from "react-dropzone";
import styles from "./Sidebar.module.css"; // Custom CSS for Sidebar
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faUpload } from '@fortawesome/free-solid-svg-icons';

const Sidebar = ({ onFileChange, onUpload, uploading, message }) => {
  const [fileName, setFileName] = useState(null); // State to hold the selected file name
  const [fileData, setFileData] = useState(null); // State to store the data extracted from the uploaded file

  const onDrop = useCallback((acceptedFiles) => {
    if (acceptedFiles && acceptedFiles.length > 0) {
      const selectedFile = acceptedFiles[0];
      setFileName(selectedFile.name); // Set the file name to state
      onFileChange(selectedFile); // Pass the file to the parent component
      extractDataFromFile(selectedFile); // Extract data from the uploaded file
    }
  }, [onFileChange]);

  const extractDataFromFile = (file) => {
    // Function to process the PDF (or other file types) and extract data
    // You can use libraries like pdf.js to extract data from PDF files
    // For this example, let's assume you're able to extract relevant data
    // and simulate it by creating a mock data array

    // Example: You can use `pdf.js` for PDF data extraction or another method for other file types.
    const mockData = [
      {
        recNum: "001",
        policyid: "POL1234",
        type: "Product A",
        summary: "This is a sample summary.",
        previewLink: "https://www.samplelink.com",
        status: "Pending",
      },
      // Add more mock rows as needed
    ];

    setFileData(mockData); // Set the extracted data
  };

  const { getRootProps, getInputProps } = useDropzone({
    onDrop,
    multiple: false, // Allow only one file
    accept: '.pdf, .xlsx, .xls, .csv, .docx', // Accepted file types
  });

  return (
    <div className={styles.sidebar}>
      <h2 className={styles.heading}>Upload Product Sheet</h2>

      <div {...getRootProps()} className={styles.dropzoneContainer}>
        <input {...getInputProps()} />
        <div className={styles.dropzone}>
          <p>Drag & Drop your file here, or click to select</p>
        </div>
      </div>

      {/* Display selected file name */}
      {fileName && (
        <div className={styles.fileNameContainer}>
          <p className={styles.fileName}>{fileName}</p>
        </div>
      )}

      <div className={styles.fileInputContainer}>
        {message && <p className={styles.message}>{message}</p>}
      </div>

      <button
        className={styles.uploadButton}
        onClick={onUpload}
        disabled={uploading}
      >
        {uploading ? (
          <div className={styles.loader}></div>
        ) : (
          <>
            <FontAwesomeIcon className={styles.icon} icon={faUpload} />
            Upload
          </>
        )}
      </button>

      {/* Pass the file data to MainContent */}
      {fileData && <MainContent message="Data loaded" rows={fileData} />}
    </div>
  );
};

export default Sidebar;



import React from 'react';
import styles from './MainContent.module.css'; // Custom CSS for MainContent
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faRotateRight } from '@fortawesome/free-solid-svg-icons';

const MainContent = ({ message, rows, handleReload }) => {
  return (
    <div className={styles.mainContent}>
      {message && <p>{message}</p>}

      {/* Table to display uploaded data */}
      <table className={styles.table}>
        <thead>
          <tr>
            <th>RecNum</th>
            <th>Policy ID</th>
            <th>Product Sheet Type</th>
            <th>Summary</th>
            <th>Preview Link</th>
            <th>Status</th>
          </tr>
        </thead>
        <tbody>
          {rows && rows.length > 0 ? (
            rows.map((row, index) => (
              <tr key={index}>
                <td>{row.recNum}</td>
                <td>{row.policyid || "Loading..."}</td>
                <td>{row.type || "Loading..."}</td>
                <td>{row.summary || "Loading..."}</td>
                <td>
                  {row.previewLink ? (
                    <a href={row.previewLink} target="_blank" rel="noopener noreferrer">
                      Preview
                    </a>
                  ) : (
                    "Pending"
                  )}
                </td>
                <td>
                  {row.status === "Pending" ? (
                    <span>
                      Pending
                      <button
                        className={styles.reloadButton}
                        onClick={() => handleReload(row.recNum)}
                      >
                        <FontAwesomeIcon icon={faRotateRight} />
                      </button>
                    </span>
                  ) : (
                    row.status
                  )}
                </td>
              </tr>
            ))
          ) : (
            <tr>
              <td colSpan="6">No data available</td>
            </tr>
          )}
        </tbody>
      </table>
    </div>
  );
};

export default MainContent;


.table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.table th,
.table td {
  padding: 12px;
  border: 1px solid #ddd;
  text-align: center;
  font-size: 14px;
}

.table th {
  background-color: #f2f2f2;
}

.reloadButton {
  background: none;
  border: none;
  color: blue;
  cursor: pointer;
  margin-left: 8px;
  font-size: 16px;
}

.reloadButton:hover {
  color: darkblue;
}
