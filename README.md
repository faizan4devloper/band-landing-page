import React, { useState } from "react";
import styles from "./Sidebar.module.css"; // Custom CSS for Sidebar
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faUpload } from '@fortawesome/free-solid-svg-icons';

const Sidebar = ({ onFileChange, onUpload, uploading }) => {
  const [dragActive, setDragActive] = useState(false);

  // Handle drag events
  const handleDrag = (e) => {
    e.preventDefault();
    e.stopPropagation();
    if (e.type === "dragenter" || e.type === "dragover") {
      setDragActive(true);
    } else if (e.type === "dragleave") {
      setDragActive(false);
    }
  };

  // Handle drop event
  const handleDrop = (e) => {
    e.preventDefault();
    e.stopPropagation();
    setDragActive(false);
    if (e.dataTransfer.files && e.dataTransfer.files[0]) {
      onFileChange(e);
    }
  };

  // File input change handler
  const handleFileInputChange = (e) => {
    onFileChange(e);
  };

  return (
    <div className={styles.sidebar}>
      <h2 className={styles.heading}>Upload Product Sheet</h2>
      <div
        className={`${styles.dropzoneContainer} ${dragActive ? styles.active : ""}`}
        onDragEnter={handleDrag}
        onDragOver={handleDrag}
        onDragLeave={handleDrag}
        onDrop={handleDrop}
      >
        <div className={styles.dropzone}>
          <p>Drag and drop your file here, or</p>
          <label htmlFor="fileUpload" className={styles.fileLabel}>
            Browse File
            <input
              type="file"
              id="fileUpload"
              onChange={handleFileInputChange}
              className={styles.fileInput}
            />
          </label>
        </div>
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


/* Dropzone Container */
.dropzoneContainer {
  width: 100%;
  padding: 20px;
  border-radius: 10px;
  border: 2px dashed #ccc;
  background-color: #f9f9f9;
  transition: all 0.3s ease;
  text-align: center;
  cursor: pointer;
}

.dropzoneContainer.active {
  background-color: #e8f5e9;
  border-color: #4caf50;
}

.dropzoneContainer.reject {
  background-color: #fdecea;
  border-color: #e57373;
}

/* Dropzone Content */
.dropzone {
  color: #555;
  font-size: 14px;
}

.dropzone p {
  margin: 0;
  font-size: 16px;
  color: #333;
}

/* Style for File Label to mimic a button */
.fileLabel {
  display: inline-block;
  padding: 10px 20px;
  background: linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  font-weight: bold;
  text-align: center;
  transition: all 0.3s ease;
  margin-top: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.fileLabel:hover {
  background: linear-gradient(90deg, #0455d0, #5e89d6, #0455d0);
  box-shadow: 0 6px 8px rgba(0, 0, 0, 0.2);
}

.fileLabel:active {
  transform: scale(0.98);
}

/* Hide the actual file input */
.fileInput {
  display: none;
}
