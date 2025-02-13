import React, { useRef, useState } from "react";
import styles from "./Sidebar.module.css";
import { useNavigate } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowLeft, faChevronLeft } from '@fortawesome/free-solid-svg-icons';

const Sidebar = ({ onFileChange, onUpload, uploading, onPolicyChange , setFileNames, toggleSidebar }) => {
  const fileInputRef = useRef(null);
  const [fileName, setFileName] = useState("");
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
      const sanitizedFileName = file.name.replace(/\s+/g, '_');
      setFileName(sanitizedFileName);
      const renamedFile = new File([file], sanitizedFileName, { type:file.type });
      onFileChange({ target: { files: [renamedFile]}});
    }
    onFileChange(event);
  };
  
  const handlePolicySelect = (e) => {
    onPolicyChange(e.target.value);
  };

  return (
    <div className={styles.sidebar}>
      <button className={styles.closeSidebarButton} onClick={toggleSidebar}>
        <FontAwesomeIcon icon={faChevronLeft} />
      </button>
      <h2 className={styles.heading}>Manage Claim</h2>
      <div className={styles.fileInputContainer} onClick={handleContainerClick}>
        <p className={styles.dropzoneText}>Click to choose a file or drag & drop</p>
        <input type="file" ref={fileInputRef} onChange={handleFileChange} className={styles.fileInput} hidden />
      </div>
      {fileName && <div className={styles.fileNameContainer}><p className={styles.fileName}>{fileName}</p></div>}
      <button className={styles.uploadButton} onClick={onUpload} disabled={uploading}>
        {uploading ? <div className={styles.loader}></div> : "Upload"}
      </button>
    </div>
  );
};

export default Sidebar;
