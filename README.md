import React, { useState } from "react";
import styles from "./Sidebar.module.css"; // Custom CSS for Sidebar
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faUpload } from '@fortawesome/free-solid-svg-icons';

const Sidebar = ({ onFileChange, onUpload, uploading }) => {
  const [dragging, setDragging] = useState(false);
  
  // Handle drag enter
  const handleDragEnter = (e) => {
    e.preventDefault();
    setDragging(true);
  };
  
  // Handle drag leave
  const handleDragLeave = (e) => {
    e.preventDefault();
    setDragging(false);
  };
  
  // Handle drop
  const handleDrop = (e) => {
    e.preventDefault();
    setDragging(false);
    const file = e.dataTransfer.files[0];
    if (file) onFileChange(file);
  };

  return (
    <div className={styles.sidebar}>
      <h2 className={styles.heading}>Upload Product Sheet</h2>
      <div 
        className={`${styles.dropzoneContainer} ${dragging ? styles.active : ''}`}
        onDragEnter={handleDragEnter}
        onDragLeave={handleDragLeave}
        onDrop={handleDrop}
      >
        <div className={styles.dropzone}>
          <FontAwesomeIcon icon={faUpload} size="2x" className={styles.icon}/>
          <p>Drag & Drop your file here</p>
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
