import React from "react";
import styles from "./Sidebar.module.css"; // Custom CSS for Sidebar
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faUpload } from '@fortawesome/free-solid-svg-icons';
import { useDropzone } from 'react-dropzone'; // Import Dropzone

const Sidebar = ({ onFileChange, onUpload, uploading }) => {
  const { getRootProps, getInputProps } = useDropzone({
    onDrop: (acceptedFiles) => {
      // Check if file is received properly
      console.log("Dropped files: ", acceptedFiles); // Debugging: See the dropped files in the console
      if (acceptedFiles && acceptedFiles.length > 0) {
        onFileChange(acceptedFiles[0]); // Pass the selected file to parent (single file)
      }
    },
    multiple: false, // Allow only one file at a time
    accept: '.xlsx, .xls, .csv', // Only accept specific file types (you can change this)
  });

  return (
    <div className={styles.sidebar}>
      <h2 className={styles.heading}>Upload Product Sheet</h2>
      <div className={styles.fileInputContainer}>
        {/* Dropzone Component: This replaces the file input */}
        <div {...getRootProps()} className={styles.dropzone}>
          <input {...getInputProps()} />
          <p className={styles.dropzoneText}>
            Drag & Drop your file here or click to browse
          </p>
        </div>
      </div>

      {/* Upload Button */}
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
