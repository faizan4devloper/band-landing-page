import React, { useState } from "react";
import { useDropzone } from "react-dropzone"; // Import react-dropzone hook
import styles from "./Sidebar.module.css"; // Custom CSS for Sidebar
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faUpload } from '@fortawesome/free-solid-svg-icons';

const Sidebar = ({ onFileChange, onUpload, uploading }) => {
  const [file, setFile] = useState(null);

  // Use the useDropzone hook from react-dropzone
  const { getRootProps, getInputProps } = useDropzone({
    accept: '.csv, .xls, .xlsx', // Allow only specific file types
    onDrop: (acceptedFiles) => {
      const uploadedFile = acceptedFiles[0];
      setFile(uploadedFile);
      onFileChange(uploadedFile); // Callback to handle the file change
    }
  });

  return (
    <div className={styles.sidebar}>
      <h2 className={styles.heading}>Upload Product Sheet</h2>
      
      {/* Dropzone container */}
      <div 
        {...getRootProps()}
        className={`${styles.dropzoneContainer} ${file ? styles.fileSelected : ''}`}
      >
        <input {...getInputProps()} />
        <div className={styles.dropzone}>
          <FontAwesomeIcon icon={faUpload} size="2x" className={styles.icon} />
          <p>{file ? `File: ${file.name}` : 'Drag & Drop your file here'}</p>
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
