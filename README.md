import React, { useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faUpload } from '@fortawesome/free-solid-svg-icons';
import styles from './Sidebar.module.css'; // Custom CSS for Sidebar

const Sidebar = ({ onFileChange, onUpload, uploading }) => {
  const [dragging, setDragging] = useState(false);
  const [file, setFile] = useState(null);

  // Handle the drag over event
  const handleDragOver = (event) => {
    event.preventDefault(); // Necessary to allow dropping
    setDragging(true); // Show the "dragging" state
  };

  // Handle the drag leave event
  const handleDragLeave = () => {
    setDragging(false); // Reset dragging state when the file leaves the area
  };

  // Handle the drop event
  const handleDrop = (event) => {
    event.preventDefault();
    setDragging(false); // Reset dragging state

    const files = event.dataTransfer.files;
    if (files && files.length > 0) {
      const file = files[0];
      console.log('File dropped:', file); // Log file for debugging
      setFile(file); // Set the file state
      onFileChange(file); // Pass the selected file to parent (optional)
    }
  };

  // Handle file input change (if the user chooses a file using the browse option)
  const handleFileChange = (event) => {
    const file = event.target.files[0];
    console.log('File selected:', file); // Log file for debugging
    setFile(file); // Set the file state
    onFileChange(file); // Pass the selected file to parent (optional)
  };

  return (
    <div className={styles.sidebar}>
      <h2 className={styles.heading}>Upload Product Sheet</h2>
      <div
        className={`${styles.fileInputContainer} ${dragging ? styles.dragging : ''}`}
        onDragOver={handleDragOver}
        onDragLeave={handleDragLeave}
        onDrop={handleDrop}
      >
        <p className={styles.dropzoneText}>
          Drag & Drop your file here or click to browse
        </p>
        <input
          type="file"
          id="fileUpload"
          onChange={handleFileChange}
          className={styles.fileInput}
        />
      </div>
      <button
        className={styles.uploadButton}
        onClick={() => onUpload(file)} // Pass file to upload handler
        disabled={uploading || !file}
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
