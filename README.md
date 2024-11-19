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
