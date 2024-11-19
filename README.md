import React from "react";
import styles from "./Sidebar.module.css"; // Custom CSS for Sidebar
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faUpload } from '@fortawesome/free-solid-svg-icons';


const Sidebar = ({ onFileChange, onUpload, uploading }) => {
  return (
    <div className={styles.sidebar}>
      <h2 className={styles.heading}>Upload Product Sheet</h2>
      <div className={styles.fileInputContainer}>
        <label htmlFor="fileUpload" className={styles.fileLabel}>
          Choose File
          <input
            type="file"
            id="fileUpload"
            onChange={onFileChange}
            className={styles.fileInput}
          />
        </label>
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
            <FontAwesomeIcon className={styles.icon} icon={faUpload}/>
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
  width: 250px;
  border-right: 1px solid rgba(0, 0, 0, 0.1);
  color: #333;
  padding: 20px;
  height: 100vh;
  overflow-y: hidden;
  /*background: linear-gradient(135deg, #ffffff, #f2f6fc);*/
  /*box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);*/
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* Heading */
.heading {
     font-size: 1.2rem;
    font-weight: bold;
    color: #0f5fdc;
    background: linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
}

/* File Input Container */
.fileInputContainer {
  margin-bottom: 15px;
  width: 100%;
  text-align: center;
}

/* File Input Label */
.fileLabel {
  border: 2px dashed #cccccc;
  padding: 5px;
  font-size: 12px;
  cursor: pointer;
  text-align: center;
  border-radius: 8px;
  background: rgba(230, 235, 245, 1);
  margin-bottom: 20px;
  transition: background-color 0.3s ease;
}

.dropzone:hover {
  background: linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%);
  color: #fff;
}

.fileLabel:hover {
  background-color: #45a049;
}

/* Hidden File Input */
.fileInput {
  display: none;
}

/* Upload Button */
.uploadButton {
  padding: 10px 20px;
  background:linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.uploadButton:disabled {
  background:linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
  cursor: not-allowed;
}

.uploadButton:hover:not(:disabled) {
  background-color: #45a049;
}

/* Loader */
.loader {
  border: 3px solid #f3f3f3;
  border-top: 3px solid #4CAF50;
  border-radius: 50%;
  width: 16px;
  height: 16px;
  animation: spin 1s linear infinite;
}

/* Loader Animation */
@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

/* Upload Button Icon */
.icon {
  margin-right: 8px;
}
