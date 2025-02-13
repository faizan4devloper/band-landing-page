import React, { useRef, useState } from "react";
import styles from "./Sidebar.module.css";
import { useNavigate } from "react-router-dom";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faAnglesLeft, faAnglesRight } from "@fortawesome/free-solid-svg-icons";

const Sidebar = ({ onFileChange, onUpload, uploading, onPolicyChange }) => {
  const fileInputRef = useRef(null);
  const [fileName, setFileName] = useState("");
  const [isSidebarOpen, setIsSidebarOpen] = useState(true);
  const [uploadSuccess, setUploadSuccess] = useState(false);
  const navigate = useNavigate();

  const handleContainerClick = () => {
    if (fileInputRef.current) {
      fileInputRef.current.click();
    }
  };

  const handleBackClick = () => {
    navigate("/claims");
  };

  const handleFileChange = (event) => {
    const file = event.target.files[0];
    if (file) {
      const sanitizedFileName = file.name.replace(/\s+/g, "_");
      setFileName(sanitizedFileName);
      const renamedFile = new File([file], sanitizedFileName, { type: file.type });
      onFileChange({ target: { files: [renamedFile] } });
    }
  };

  const handleUpload = async () => {
    await onUpload();
    setUploadSuccess(true);
    setTimeout(() => {
      setIsSidebarOpen(false);
      setUploadSuccess(false);
    }, 2000);
  };

  return (
    <div className={`${styles.sidebar} ${!isSidebarOpen ? styles.sidebarHidden : ""}`}>
      <button
        className={styles.toggleButton}
        onClick={() => setIsSidebarOpen(!isSidebarOpen)}
      >
        <FontAwesomeIcon icon={isSidebarOpen ? faAnglesLeft : faAnglesRight} />
      </button>

      {isSidebarOpen && (
        <>
          <h2 className={styles.heading}>Manage Claim</h2>

          <div className={styles.fileInputContainer} onClick={handleContainerClick}>
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

          <button className={styles.uploadButton} onClick={handleUpload} disabled={uploading}>
            {uploading ? <div className={styles.loader}></div> : "Upload"}
          </button>

          {uploadSuccess && <p className={styles.successMessage}>Upload Successful!</p>}
        </>
      )}
    </div>
  );
};

export default Sidebar;
