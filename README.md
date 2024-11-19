import React, { useState, useCallback } from "react";
import { useDropzone } from "react-dropzone";
import styles from "./Sidebar.module.css";
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faUpload } from '@fortawesome/free-solid-svg-icons';

const Sidebar = ({ onFileChange, onUpload, uploading, message }) => {
  const [selectedFile, setSelectedFile] = useState(null);
  const [errorMessage, setErrorMessage] = useState(""); // State to store error messages

  const onDrop = useCallback(
    (acceptedFiles, fileRejections) => {
      setErrorMessage(""); // Clear previous errors
      if (fileRejections.length > 0) {
        // Handle invalid file rejection
        setErrorMessage("Invalid file type or size. Please try again.");
        return;
      }
      if (acceptedFiles && acceptedFiles.length > 0) {
        const file = acceptedFiles[0];
        setSelectedFile(file);
        onFileChange(file); // Pass the file to the parent
      }
    },
    [onFileChange]
  );

  const handleUpload = () => {
    if (selectedFile) {
      onUpload(selectedFile);
    }
  };

  const { getRootProps, getInputProps, isDragActive, isDragReject } = useDropzone({
    onDrop,
    multiple: false,
    accept: { 
      'application/pdf': ['.pdf'], 
      'application/vnd.ms-excel': ['.xls', '.xlsx', '.csv'], 
      'application/vnd.openxmlformats-officedocument.wordprocessingml.document': ['.docx'] 
    },
    maxSize: 10 * 1024 * 1024, // 10 MB file size limit
  });

  return (
    <div className={styles.sidebar}>
      <h2 className={styles.heading}>Upload Product Sheet</h2>

      <div
        {...getRootProps()}
        className={`${styles.dropzoneContainer} ${
          isDragActive ? styles.active : isDragReject ? styles.reject : ""
        }`}
      >
        <input {...getInputProps()} />
        <div className={styles.dropzone}>
          <p>
            {isDragReject
              ? "File type not accepted"
              : isDragActive
              ? "Drop the file here..."
              : "Drag & Drop your file here, or click to select"}
          </p>
        </div>
      </div>

      {errorMessage && <p className={styles.errorMessage}>{errorMessage}</p>}
      {selectedFile && (
        <div className={styles.fileNameContainer}>
          <p className={styles.fileName}>{selectedFile.name}</p>
        </div>
      )}

      {message && <p className={styles.message}>{message}</p>}

      <button
        className={styles.uploadButton}
        onClick={handleUpload}
        disabled={uploading || !selectedFile}
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


/* Add visual feedback for drag events */
.dropzoneContainer.active {
  background-color: #e3f7e4;
  border-color: #4caf50;
}

.dropzoneContainer.reject {
  background-color: #fdecea;
  border-color: #e57373;
}

.errorMessage {
  color: #e57373;
  font-size: 12px;
  margin-top: 10px;
}
