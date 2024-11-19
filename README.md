import React, { useState, useCallback } from "react";
import { useDropzone } from "react-dropzone";
import styles from "./Sidebar.module.css"; // Custom CSS for Sidebar
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faUpload } from '@fortawesome/free-solid-svg-icons';

const Sidebar = ({ onFileChange, onUpload, uploading, message }) => {
  const [selectedFile, setSelectedFile] = useState(null); // State to hold the selected file object

  const onDrop = useCallback((acceptedFiles) => {
    if (acceptedFiles && acceptedFiles.length > 0) {
      const file = acceptedFiles[0];
      setSelectedFile(file); // Store the full file object
      onFileChange(file); // Pass the file to the parent component
    }
  }, [onFileChange]);

  const handleUpload = () => {
    if (selectedFile) {
      onUpload(selectedFile); // Ensure the selected file is passed to the upload function
    } else {
      console.error("No file selected for upload.");
    }
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
      {selectedFile && (
        <div className={styles.fileNameContainer}>
          <p className={styles.fileName}>{selectedFile.name}</p>
        </div>
      )}

      <div className={styles.fileInputContainer}>
        {message && <p className={styles.message}>{message}</p>}
      </div>

      <button
        className={styles.uploadButton}
        onClick={handleUpload}
        disabled={uploading || !selectedFile} // Disable if no file or upload is in progress
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
  width: 250px;
  border-right: 1px solid rgba(0, 0, 0, 0.1);
  color: #333;
  padding: 20px;
  height: 100vh;
  overflow-y: hidden;
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
  margin-bottom: 20px;
}

/* Dropzone Container */
.dropzoneContainer {
  /*width: 100%;*/
  display: flex;
  justify-content: center;
  align-items: center;
  /*padding: 20px;*/
  border-radius: 10px;
  border: 2px dashed #ccc;
  background-color: #f9f9f9;
  transition: all 0.3s ease;
  cursor: pointer;
}

/* Dropzone Styling */
.dropzone {
  padding: 20px;
  text-align: center;
  color: #555;
  font-size: 14px;
  border-radius: 8px;
  width: 100%;
}

.dropzone:hover {
  background-color: #e8f5e9;
  border-color: #4caf50;
  color: #4caf50;
}

.dropzone p {
  margin: 0;
}

/* File Name Container */
.fileNameContainer {
  margin-top: 15px;
  background-color: #f3f3f3;
  padding: 10px;
  border-radius: 8px;
  width: 100%;
  text-align: center;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

/* File Name Text */
.fileName {
  font-size: 14px;
  color: #333;
  font-weight: 600;
  margin: 0;
  word-wrap: break-word;
  text-overflow: ellipsis;
  overflow: hidden;
  white-space: nowrap;
}

/* File Input Container */
.fileInputContainer {
  margin-top: 15px;
  width: 100%;
  text-align: center;
}

/* Upload Button */
.uploadButton {
  padding: 10px 20px;
  background: linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 20px;
}

.uploadButton:disabled {
  background: linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
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

/* Message below file input */
.message {
  font-size: 12px;
  color: #555;
  margin-top: 10px;
}
