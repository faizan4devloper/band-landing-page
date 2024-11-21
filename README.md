import React from 'react';
import styles from './MainContent.module.css'; // Custom CSS for MainContent
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faRotateRight } from '@fortawesome/free-solid-svg-icons';


const MainContent = ({ message, rows, handleReload }) => {
  return (
    <div className={styles.mainContent}>
      {message && <p>{message}</p>}
      
      {/* Table to display uploaded data */}
      <table className={styles.table}>
        <thead>
          <tr>
            <th>RecNum</th>
            <th>Policy ID</th>
            <th>Product Sheet Type</th>
            <th>Summary</th>
            <th>Preview Link</th>
            <th>Status</th>
          </tr>
        </thead>
        <tbody>
          {rows && rows.length > 0 ? (
            rows.map((row, index) => (
              <tr key={index}>
                <td>{row.recNum}</td>
                <td>{row.policyid || "Loading..."}</td>
                <td>{row.type || "Loading..."}</td>
                <td>{row.summary || "Loading..."}</td>
                <td>
                  {row.previewLink ? (
                    <a href={row.previewLink} target="_blank" rel="noopener noreferrer">
                      Preview
                    </a>
                  ) : (
                    "Pending"
                  )}
                </td>
                <td>
                  {row.status === "Pending" ? (
                    <span>
                      Pending
                      <button
                        className={styles.reloadButton}
                        onClick={() => handleReload(row.recNum)}
                      >
<FontAwesomeIcon icon={faRotateRight} />                      </button>
                    </span>
                  ) : (
                    row.status
                  )}
                </td>
              </tr>
            ))
          ) : (
            <tr>
              <td colSpan="6">No data available</td>
            </tr>
          )}
        </tbody>
      </table>
    </div>
  );
};

export default MainContent;



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
