import React, { useRef } from "react";
import styles from "./Sidebar.module.css"; // Custom CSS for Sidebar
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faUpload } from '@fortawesome/free-solid-svg-icons';

const Sidebar = ({ onFileChange, onUpload, uploading }) => {
  // Create a ref for the file input
  const fileInputRef = useRef(null);

  // Trigger the file input when the fileInputContainer is clicked
  const handleContainerClick = () => {
    if (fileInputRef.current) {
      fileInputRef.current.click(); // Programmatically click the hidden file input
    }
  };

  return (
    <div className={styles.sidebar}>
      <h2 className={styles.heading}>Upload Product Sheet</h2>
      <div
        className={styles.fileInputContainer}
        onClick={handleContainerClick} // Trigger the file input click when container is clicked
      >
        <p className={styles.dropzoneText}>Click to choose a file or drag & drop</p>
        <input
          type="file"
          ref={fileInputRef} // Attach ref to the file input
          onChange={onFileChange}
          className={styles.fileInput}
          style={{ display: "none" }} // Hide the file input
        />
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
            <FontAwesomeIcon className={styles.icon} icon={faUpload} />
            Upload
          </>
        )}
      </button>
    </div>
  );
};

export default Sidebar;
