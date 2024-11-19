import React from "react";
import styles from "./Sidebar.module.css"; // Custom CSS for Sidebar
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { useDropzone } from 'react-dropzone';
import { faUpload } from '@fortawesome/free-solid-svg-icons';


const Sidebar = ({ onFileChange, onUpload, uploading }) => {
  
 
const { getRootProps, getInputProps } = useDropzone({
    onDrop: (acceptedFiles) => {
      // Handle file selection here
      onFileChange(acceptedFiles);
    },
    multiple: false, // Allow only one file at a time
  });

  
  return (
    <div className={styles.sidebar}>
      <h2 className={styles.heading}>Upload Product Sheet</h2>
      
<div className={styles.fileInputContainer}>
  <div {...getRootProps()} className={styles.dropzone}>
    <input {...getInputProps()} />
    <p className={styles.dropzoneText}>Drag & Drop your file here or click to browse</p>
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
            <FontAwesomeIcon className={styles.icon} icon={faUpload}/>
            Upload
          </>
        )}
      </button>
    </div>
  );
};

export default Sidebar;
