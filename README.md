import React, { useRef, useState } from "react";
import styles from "./Sidebar.module.css";
import { useNavigate } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowLeft } from '@fortawesome/free-solid-svg-icons';

const Sidebar = ({ onFileChange, onUpload, uploading, onPolicyChange }) => {
  const fileInputRef = useRef(null);
  const [fileName, setFileName] = useState("");
  const [selectedPolicy, setSelectedPolicy] = useState("PS391481"); // Set default policy
  const navigate = useNavigate();

  const handleContainerClick = () => {
    if (fileInputRef.current) {
      fileInputRef.current.click();
    }
  };

  const handleBackClick = () => {
    navigate("/datatable");
  };

  const handleFileChange = (event) => {
    const file = event.target.files[0];
    if (file) {
      const sanitizedFileName = file.name.replace(/\s+/g, '_');
      setFileName(sanitizedFileName);
      const renamedFile = new File([file], sanitizedFileName, { type: file.type });
      onFileChange({ target: { files: [renamedFile] } });
    }
  };

  const handlePolicySelect = (e) => {
    setSelectedPolicy(e.target.value); // Update the selected policy state
    onPolicyChange(e.target.value); // Call the onPolicyChange function
  };

  // Array of policy options
  const options = [
    { value: "PS391481", label: "Cancer - PS391481" },
    { value: "PS672908", label: "Heart - PS672908" },
    { value: "PS000000", label: "Please select a policy...", disabled: true } // Default option
  ];

  return (
    <div className={styles.sidebar}>
      <button className={styles.backButton} onClick={handleBackClick}>
        <FontAwesomeIcon icon={faArrowLeft} />
      </button>
      <h2 className={styles.heading}>Manage Claim</h2>
      
      <select 
        id="psDropdown" 
        onChange={handlePolicySelect} 
        value={selectedPolicy} // Set the value of the select element
        className={styles.policySelect}
      >
        {options.map((option) => (
          <option 
            key={option.value} 
            value={option.value} 
            disabled={option.disabled}
          >
            {option.label}
          </option>
        ))}
      </select>

      <div
        className={styles.fileInputContainer}
        onClick={handleContainerClick}
      >
        <p className={styles.dropzoneText}>Click to choose a file or drag & drop</p>
        <input
          type="file"
          ref={fileInputRef}
          onChange={handleFileChange}
          className={styles.fileInput}
          style={{ display: "none" }}
        />
      </div>

      {fileName && (
        <div className={styles.fileNameContainer}>
          <p className={styles.fileName}>{fileName}</p>
        </div>
      )}

      <button
        className={styles.uploadButton}
        onClick={onUpload}
        disabled={uploading}
      >
        {uploading ? (
          <div className={styles.loader}></div>
        ) : (
          "Upload"
        )}
      </button>
    </div>
  );
};

export default Sidebar;




/* Sidebar Container */
.sidebar {
  width: 250px;
  background-color: #f4f4f9;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

/* Back Button */
.backButton {
  background: transparent;
  border: none;
  font-size: 1.5rem;
  color: #333;
  cursor: pointer;
  transition: all 0.3s ease;
}

.backButton:hover {
  color: #0056b3;
}

/* Heading */
.heading {
  font-size: 1.5rem;
  font-weight: bold;
  margin-bottom: 20px;
  color: #333;
}

/* Dropdown (Select) Styling */
.policySelect {
  width: 100%;
  padding: 10px;
  font-size: 1rem;
  margin-bottom: 20px;
  border-radius: 8px;
  border: 1px solid #ddd;
  background-color: #fff;
  color: #333;
  transition: border-color 0.3s ease;
}

.policySelect:focus {
  border-color: #0056b3;
  outline: none;
}

/* Option Styling */
.policySelect option {
  padding: 10px;
  font-size: 1rem;
  color: #333;
  background-color: #fff;
}

/* File Input Container */
.fileInputContainer {
  width: 100%;
  padding: 20px;
  border: 2px dashed #ccc;
  border-radius: 8px;
  background-color: #f9f9f9;
  margin-bottom: 20px;
  text-align: center;
  cursor: pointer;
  transition: background-color 0.3s ease, border-color 0.3s ease;
}

.fileInputContainer:hover {
  background-color: #f0f0f0;
  border-color: #0056b3;
}

/* File Name Display */
.fileNameContainer {
  width: 100%;
  padding: 10px;
  background-color: #e9ecef;
  border-radius: 8px;
  margin-bottom: 20px;
  text-align: center;
}

.fileName {
  font-size: 1rem;
  color: #333;
}

/* Upload Button */
.uploadButton {
  width: 100%;
  padding: 12px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.uploadButton:hover {
  background-color: #0056b3;
}

.uploadButton:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

/* Loader */
.loader {
  width: 25px;
  height: 25px;
  border: 3px solid #f3f3f3;
  border-top: 3px solid #007bff;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

/* Spinner Animation */
@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

/* Hover and Active Effects */
.policySelect option:disabled {
  color: #ccc;
  background-color: #f5f5f5;
}

.policySelect option:hover {
  background-color: #f0f0f0;
}

.uploadButton:active {
  transform: scale(0.98);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}
