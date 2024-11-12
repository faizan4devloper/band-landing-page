import React, { useState } from 'react';
import { useDropzone } from 'react-dropzone';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowUpFromBracket } from '@fortawesome/free-solid-svg-icons'; // Import FontAwesome left arrow icon
import styles from './Sidebar.module.css';

// Modal component for preview
const FilePreviewModal = ({ fileName, closeModal }) => {
  return (
    <div className={styles.modalOverlay} onClick={closeModal}>
      <div className={styles.modalContent} onClick={(e) => e.stopPropagation()}>
        <h2>File Preview</h2>
        <p>Preview for: {fileName}</p>
        {/* You can add more file-specific preview logic here, e.g., image preview */}
        <button onClick={closeModal} className={styles.closeButton}>Close</button>
      </div>
    </div>
  );
};

const Sidebar = () => {
  const [fileName, setFileName] = useState(null);
  const [showModal, setShowModal] = useState(false); // State for modal visibility

  const onDrop = (acceptedFiles) => {
    setFileName(acceptedFiles[0].name); // Store the file name of the first file
  };

  const { getRootProps, getInputProps } = useDropzone({ onDrop });

  const openModal = () => {
    setShowModal(true); // Show modal
  };

  const closeModal = () => {
    setShowModal(false); // Close modal
  };

  return (
    <div className={styles.sidebar}>
      <h2 className={styles.sidebarHeader}>Document Uploaded</h2>
      <div {...getRootProps()} className={styles.dropzone}>
        <input {...getInputProps()} />
        <p>Drag & Drop, or click to select files</p>
        <FontAwesomeIcon icon={faArrowUpFromBracket} className={styles.uploadIcon} />
      </div>
      {fileName && (
        <p className={styles.fileName} onClick={openModal}>
          {fileName} {/* Clicking on file name will open modal */}
        </p>
      )}

      {/* Modal for preview */}
      {showModal && <FilePreviewModal fileName={fileName} closeModal={closeModal} />}
    </div>
  );
};

export default Sidebar;




/* Modal Overlay */
.modalOverlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

/* Modal Content */
.modalContent {
  background: white;
  padding: 20px;
  border-radius: 10px;
  text-align: center;
  width: 400px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

/* Close button style */
.closeButton {
  margin-top: 20px;
  padding: 8px 16px;
  background-color: #4080f5;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.closeButton:hover {
  background-color: #572ac2;
}
