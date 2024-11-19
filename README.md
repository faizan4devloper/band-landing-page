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
