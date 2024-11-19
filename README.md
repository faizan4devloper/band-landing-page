import React, { useState, useRef } from "react";
import styles from "./Sidebar.module.css"; // Custom CSS for Sidebar
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faUpload } from '@fortawesome/free-solid-svg-icons';

const Sidebar = ({ onFileChange, onUpload, uploading }) => {
  const [selectedFileName, setSelectedFileName] = useState(""); // State to hold selected file name
  const fileInputRef = useRef(null);

  // Trigger the file input when the fileInputContainer is clicked
  const handleContainerClick = () => {
    if (fileInputRef.current) {
      fileInputRef.current.click(); // Programmatically click the hidden file input
    }
  };

  // Handle file selection and update file name
  const handleFileChange = (event) => {
    const file = event.target.files[0];
    if (file) {
      setSelectedFileName(file.name); // Update state with selected file name
      onFileChange(file); // Call the provided onFileChange function (if needed)
    }
  };

  return (
    <div className={styles.sidebar}>
      <h2 className={styles.heading}>Upload Product Sheet</h2>
      <div
        className={styles.fileInputContainer}
        onClick={handleContainerClick} // Trigger the file input click when container is clicked
      >
        <p className={styles.dropzoneText}>
          {selectedFileName ? (
            `Selected file: ${selectedFileName}` // Display selected file name
          ) : (
            "Click to choose a file or drag & drop"
          )}
        </p>
        <input
          type="file"
          ref={fileInputRef} // Attach ref to the file input
          onChange={handleFileChange}
          className={styles.fileInput}
          style={{ display: "none" }} // Hide the file input
        />
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
    </div>
  );
};

export default Sidebar;


/* Sidebar.module.css */
.fileInputContainer {
  padding: 20px;
  border: 2px dashed #ccc;
  border-radius: 8px;
  text-align: center;
  cursor: pointer;
  transition: background-color 0.3s ease;
  min-height: 100px; /* Ensures enough space for text */
}

.fileInputContainer:hover {
  background-color: #e9f7ff; /* Light blue when hovering over the container */
}

.dropzoneText {
  color: #888;
  font-size: 16px;
  font-weight: 500;
}

.selectedFileText {
  font-size: 16px;
  color: #2d2d2d; /* Dark color for the file name */
  font-weight: 600;
  margin-top: 10px;
}

.fileInput {
  display: none; /* File input is hidden */
}

.uploadButton {
  margin-top: 20px;
  padding: 10px 20px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 16px;
}

.uploadButton:disabled {
  background-color: #ccc;
}

.loader {
  border: 4px solid #f3f3f3;
  border-top: 4px solid #3498db;
  border-radius: 50%;
  width: 20px;
  height: 20px;
  animation: spin 2s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
