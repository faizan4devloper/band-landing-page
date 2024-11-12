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
    setFile(acceptedFiles[0]);
  };

  const { getRootProps, getInputProps } = useDropzone({ onDrop });

  const openModal = () => {
    setShowModal(true);
  };

  const closeModal = () => {
    setShowModal(false);
  };

  const removeFile = () => {
    setFile(null);
    closeModal();
  };

  return (
    <div className={styles.sidebar}>
      <h2 className={styles.sidebarHeader}>Upload Document</h2>
      <div {...getRootProps()} className={styles.dropzone}>
        <input {...getInputProps()} />
        <p>Drag & Drop, or click to select a file</p>
        <FontAwesomeIcon icon={faArrowUpFromBracket} className={styles.uploadIcon} />
      </div>
      {file && (
        <div className={styles.fileContainer}>
          <p className={styles.fileName} onClick={openModal}>
            {file.name} <span className={styles.previewLabel}>(Preview)</span>
          </p>
          <FontAwesomeIcon
            icon={faTimes}
            className={styles.removeIcon}
            onClick={removeFile}
          />
        </div>
      )}
      {showModal && <FilePreviewModal file={file} closeModal={closeModal} />}
    </div>
  );
};

export default Sidebar;



.sidebar {
  width: 250px;
  padding: 20px;
  height: 100%;
  border-right: 1px solid rgba(0, 0, 0, 0.1);
  background: linear-gradient(135deg, #f8f9fb 0%, #eceff4 100%);
  color: #333;
  overflow-y: hidden;
  box-shadow: 2px 0 10px rgba(0, 0, 0, 0.1);
}

.sidebarHeader {
  font-size: 1.2rem;
  text-align: center;
  font-weight: bold;
  color: #5f1ec1;
  margin-bottom: 25px;
  border-bottom: 2px solid #e1e4f0;
  padding-bottom: 8px;
}

.dropzone {
  border: 2px dashed #cccccc;
  padding: 20px;
  font-size: 14px;
  cursor: pointer;
  text-align: center;
  border-radius: 8px;
  background: rgba(230, 235, 245, 1);
  margin-bottom: 20px;
  transition: background-color 0.3s ease, border-color 0.3s ease;
}

.dropzone:hover {
  background: linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%);
  border-color: #5f1ec1;
  color: #fff;
}

.uploadIcon {
  font-size: 18px;
  color: #5f1ec1;
  margin-top: 10px;
}

.fileContainer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 15px;
  padding: 10px;
  border-radius: 8px;
  border: 1px solid #f0f0f0;
  background: #fdfdfd;
  box-shadow: 0 1px 5px rgba(0, 0, 0, 0.1);
  cursor: pointer;
  transition: background-color 0.3s, color 0.3s;
}

.fileContainer:hover {
  background: linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%);
  color: #fff;
}

.fileName {
  flex: 1;
  text-align: left;
  font-size: 14px;
  color: #333;
  margin-right: 8px;
}

.previewLabel {
  font-size: 12px;
  font-weight: bold;
  color: #5f1ec1;
  margin-left: 5px;
}

.removeIcon {
  color: #d9534f;
  font-size: 14px;
  cursor: pointer;
  transition: color 0.3s;
}

.removeIcon:hover {
  color: #fff;
}

