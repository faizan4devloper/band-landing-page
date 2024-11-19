import React from 'react';
import styles from './Sidebar.module.css'; // Custom CSS for Sidebar

const Sidebar = ({ onFileChange, onUpload, uploading }) => {
  return (
    <div className={styles.sidebar}>
      <h3>Upload Product Sheet</h3>
      <input type="file" onChange={onFileChange} />
      <button
        className={styles.uploadButton}
        onClick={onUpload}
        disabled={uploading}
      >
        {uploading ? "Uploading..." : "Upload"}
      </button>
    </div>
  );
};

export default Sidebar;




    .sidebar {
        width: 250px;
      border-right: 1px solid rgba(0, 0, 0, 0.1);
      color: #333;
      padding: 20px;
      height: 100vh;
      overflow-y: hidden;
    }
    
    .uploadButton {
      margin-top: 10px;
      padding: 10px 20px;
      background-color: #4CAF50;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    
    .uploadButton:disabled {
      background-color: #ccc;
      cursor: not-allowed;
    }
