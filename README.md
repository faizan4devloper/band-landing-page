import React, { useState } from 'react';
import { useDropzone } from 'react-dropzone';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowUpFromBracket, faTimes, faArrowLeft } from '@fortawesome/free-solid-svg-icons';
import styles from './Sidebar.module.css';
import FilePreviewModal from './FilePreviewModal'; // Import the FilePreviewModal
import { useNavigate } from 'react-router-dom';

const Sidebar = ({ onFileUpload, onFileRemove }) => {
  const [file, setFile] = useState(null);
  const [showModal, setShowModal] = useState(false);  // State to control modal visibility
  const navigate = useNavigate();

  // Function to handle file drop and set the file
  const onDrop = (acceptedFiles) => {
    const uploadedFile = acceptedFiles[0];
    setFile(uploadedFile);
    onFileUpload(uploadedFile); // Notify JobPage that a file has been uploaded
  };

  const { getRootProps, getInputProps } = useDropzone({ onDrop });

  // Function to remove the uploaded file
  const removeFile = () => {
    setFile(null);
    onFileRemove(); // Notify parent component to clear data
  };

  // Function to open the preview modal
  const openModal = () => setShowModal(true);

  // Function to close the preview modal
  const closeModal = () => setShowModal(false);

  // Function to navigate back
  const handleBackClick = () => {
    navigate(-1); // Navigate back to the previous page
  };

  return (
    <div className={styles.sidebar}>
      <button className={styles.backButton} onClick={handleBackClick}>
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
          <p className={styles.fileName} onClick={openModal} role="button" tabIndex="0">
            {file.name}
          </p>
          <FontAwesomeIcon icon={faTimes} className={styles.removeIcon} onClick={removeFile} />
        </div>
      )}

      {/* Show the preview modal if the file is selected */}
      {showModal && file && (
        <FilePreviewModal
          file={file}
          closeModal={closeModal}
        />
      )}
    </div>
  );
};

export default Sidebar;
