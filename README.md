import React, { useState } from "react";
import styles from "./Sidebar.module.css"; // Custom CSS for Sidebar
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { useDropzone } from 'react-dropzone';
import { faUpload } from '@fortawesome/free-solid-svg-icons';

const Sidebar = ({ onFileChange, onUpload, uploading }) => {
  // Add a local state to track the selected file for debugging
  const [selectedFile, setSelectedFile] = useState(null);

  const { getRootProps, getInputProps } = useDropzone({
    onDrop: (acceptedFiles) => {
      // Handle file selection here
      console.log("Selected file:", acceptedFiles[0]); // Log the selected file for debugging
      setSelectedFile(acceptedFiles[0]); // Set the file to the local state
      onFileChange(acceptedFiles); // Call the passed onFileChange function from the parent component
    },
    multiple: false, // Allow only one file at a time
    accept: '.xlsx, .xls, .csv', // Specify accepted file types (if needed)
  });

  return (
    <div className={styles.sidebar}>
      <h2 className={styles.heading}>Upload Product Sheet</h2>
      
      <div className={styles.fileInputContainer}>
        <div {...getRootProps()} className={styles.dropzone}>
          <input {...getInputProps()} />
          <p className={styles.dropzoneText}>
            {selectedFile ? `File: ${selectedFile.name}` : "Drag & Drop your file here or click to browse"}
          </p>
        </div>
      </div>

      <button
        className={styles.uploadButton}
        onClick={onUpload} // Ensure onUpload is defined in the parent component
        disabled={uploading || !selectedFile} // Disable the button if no file is selected
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
