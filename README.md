import React from "react";
import styles from "./Sidebar.module.css"; // Custom CSS for Sidebar
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faUpload, faTrashAlt } from '@fortawesome/free-solid-svg-icons';

const Sidebar = ({
  onFileChange,
  onUpload,
  uploading,
  fileName,
  onRemoveFile,
}) => {
  return (
    <div className={styles.sidebar}>
      <h2 className={styles.heading}>Upload Product Sheet</h2>
      
      <div
        className={styles.dropzoneContainer}
        onClick={() => document.getElementById("fileUpload").click()}
      >
        <div className={styles.dropzone}>
          <p>Drag & Drop or Click to Upload</p>
        </div>
        <input
          type="file"
          id="fileUpload"
          onChange={onFileChange}
          className={styles.fileInput}
        />
      </div>
      
      {fileName && (
        <div className={styles.fileNameContainer}>
          <span className={styles.fileName}>{fileName}</span>
          <FontAwesomeIcon
            className={styles.removeIcon}
            icon={faTrashAlt}
            onClick={onRemoveFile}
          />
        </div>
      )}
      
      <button
        className={styles.uploadButton}
        onClick={onUpload}
        disabled={uploading || !fileName}
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



/* Sidebar Container */
.sidebar {
  width: 300px;
  border-right: 1px solid rgba(0, 0, 0, 0.1);
  color: #333;
  padding: 20px;
  height: 100vh;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* Dropzone Container */
.dropzoneContainer {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 10px;
  border: 2px dashed #ccc;
  background-color: #f9f9f9;
  padding: 20px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.dropzoneContainer:hover {
  background-color: #e8f5e9;
  border-color: #4caf50;
}

.dropzone p {
  font-size: 14px;
  color: #555;
  margin: 0;
}

/* File Name Container */
.fileNameContainer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 15px;
  background-color: #f3f3f3;
  padding: 10px;
  border-radius: 8px;
  width: 100%;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

/* File Name Text */
.fileName {
  font-size: 14px;
  color: #333;
  font-weight: 600;
  word-wrap: break-word;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

/* Remove Icon */
.removeIcon {
  color: #ff4d4d;
  cursor: pointer;
  margin-left: 10px;
  transition: transform 0.3s ease, color 0.3s ease;
}

.removeIcon:hover {
  color: #d32f2f;
  transform: scale(1.2);
}

/* File Input */
.fileInput {
  display: none;
}
