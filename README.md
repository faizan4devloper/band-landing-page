import React, { useRef, useState } from "react";
import styles from "./Sidebar.module.css";

const Sidebar = ({ onFileChange, onUpload, uploading }) => {
  const fileInputRef = useRef(null);
  const [fileName, setFileName] = useState("");

  // Trigger the file input when the container is clicked
  const handleContainerClick = () => {
    if (fileInputRef.current) {
      fileInputRef.current.click();
    }
  };

  // Handle file selection and store the file name
  const handleFileChange = (event) => {
    const file = event.target.files[0];
    if (file) {
      setFileName(file.name);
    }
    onFileChange(event); // Pass the file to the parent component
  };

  return (
    <div className={styles.sidebar}>
      <h2 className={styles.heading}>Upload Product Sheet</h2>

      {/* File Input Container */}
      <div
        className={styles.fileInputContainer}
        onClick={handleContainerClick}
      >
        <p className={styles.dropzoneText}>Click to choose a file or drag & drop</p>
        <input
          type="file"
          ref={fileInputRef}
          onChange={handleFileChange}
          className={styles.fileInput}
          style={{ display: "none" }}
        />
      </div>

      {/* File Name Display */}
      {fileName && (
        <div className={styles.fileNameContainer}>
          <p className={styles.fileName}>{fileName}</p>
        </div>
      )}

      {/* Upload Button */}
      <button
        className={styles.uploadButton}
        onClick={onUpload}
        disabled={uploading}
      >
        {uploading ? (
          <div className={styles.loader}></div>
        ) : (
          "Upload"
        )}
      </button>
    </div>
  );
};

export default Sidebar;

/* File Name Container */
.fileNameContainer {
  margin-top: 15px;
  background-color: #f3f3f3;
  border-radius: 8px;
  padding: 10px;
  width: 100%;
  text-align: center;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

/* File Name Text */
.fileName {
  font-size: 14px;
  color: #333;
  font-weight: 600;
  word-wrap: break-word;
  text-overflow: ellipsis;
  overflow: hidden;
  white-space: nowrap;
}
