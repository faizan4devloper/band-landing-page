import React, { useState } from 'react';
import { useDropzone } from 'react-dropzone';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowUpFromBracket, faTimes, faArrowLeft } from '@fortawesome/free-solid-svg-icons';
import styles from './Sidebar.module.css';

const Sidebar = ({ onFileUpload }) => {
  const [file, setFile] = useState(null);

  const onDrop = (acceptedFiles) => {
    setFile(acceptedFiles[0]);
    onFileUpload(); // Notify JobPage that a file has been uploaded
  };

  const { getRootProps, getInputProps } = useDropzone({ onDrop });

  const removeFile = () => {
    setFile(null);
  };

  return (
    <div className={styles.sidebar}>
      <button className={styles.backButton} onClick={() => window.history.back()}>
        <FontAwesomeIcon icon={faArrowLeft} />
      </button>
      <h3 className={styles.sidebarHeader}>Document Uploaded</h3>
      <div {...getRootProps()} className={styles.dropzone}>
        <input {...getInputProps()} />
        <p>Drag & Drop, or click to select files</p>
        <FontAwesomeIcon icon={faArrowUpFromBracket} className={styles.uploadIcon} />
      </div>
      {file && (
        <div className={styles.fileContainer}>
          <p className={styles.fileName}>{file.name}</p>
          <FontAwesomeIcon icon={faTimes} className={styles.removeIcon} onClick={removeFile} />
        </div>
      )}
    </div>
  );
};

export default Sidebar;
