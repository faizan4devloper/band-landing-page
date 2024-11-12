import React, { useState } from 'react';
import { useDropzone } from 'react-dropzone';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowUpFromBracket, faTimes } from '@fortawesome/free-solid-svg-icons';
import styles from './Sidebar.module.css';
import FilePreviewModal from './FilePreviewModal';

const Sidebar = () => {
  const [file, setFile] = useState(null);
  const [showModal, setShowModal] = useState(false);

  const onDrop = (acceptedFiles) => {
    setFile(acceptedFiles[0]); // Store the first file object
  };

  const { getRootProps, getInputProps } = useDropzone({ onDrop });

  const openModal = () => {
    setShowModal(true); // Show modal
  };

  const closeModal = () => {
    setShowModal(false); // Close modal
  };

  const removeFile = () => {
    setFile(null); // Remove the file
    closeModal(); // Close modal if open
  };

  return (
    <div className={styles.sidebar}>
      <h2 className={styles.sidebarHeader}>Document Uploaded</h2>
      <div {...getRootProps()} className={styles.dropzone}>
        <input {...getInputProps()} />
        <p>Drag & Drop, or click to select files</p>
        <FontAwesomeIcon icon={faArrowUpFromBracket} className={styles.uploadIcon} />
      </div>
      {file && (
        <div className={styles.fileContainer}>
          <p className={styles.fileName} onClick={openModal}>
            {file.name} {/* Clicking on file name will open modal */}
          </p>
          <FontAwesomeIcon
            icon={faTimes}
            className={styles.removeIcon}
            onClick={removeFile}
          />
        </div>
      )}

      {/* Modal for preview */}
      {showModal && <FilePreviewModal file={file} closeModal={closeModal} />}
    </div>
  );
};

export default Sidebar;




.sidebar {
  width: 250px;
  border-right: 1px solid rgba(0, 0, 0, 0.1);
  color: #333;
  padding: 20px;
  height: 100%;
  overflow-y: hidden;
}

.sidebarHeader {
  font-size: 1rem;
  text-align: center;
  font-weight: bold;
  margin-bottom: 20px;
}

.dropzone {
  border: 2px dashed #cccccc;
  padding: 5px;
  font-size: 12px;
  cursor: pointer;
  text-align: center;
  border-radius: 8px;
  background: rgba(230, 235, 245, 1);
  margin-bottom: 20px;
  transition: background-color 0.3s ease;
}

.dropzone:hover {
  background: linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%);
  color: #fff;
}

.fileContainer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 10px;
  font-size: 14px;
  color: #333;
  cursor: pointer;
  border: 1px solid #f0f0f0;
  padding: 8px;
  border-radius: 5px;
  transition: background-color 0.3s;
}

.fileContainer:hover {
  background-color: rgba(95, 30, 193, 0.1);
}

.fileName {
  flex: 1;
  text-align: left;
  margin-right: 8px;
  font-weight: bold;
}

.uploadIcon {
  font-size: 18px;
}

.removeIcon {
  color: #888;
  cursor: pointer;
  transition: color 0.3s;
}

.removeIcon:hover {
  color: #ff4d4f;
}
