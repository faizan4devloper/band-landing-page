  resume_mapping = {
        "scenario_1": "DataEngineerDataScientistResume.pdf",
        "scenario_2": "NonTechDataEngineerResume.pdf"
    }

test_event = {
"resume_identifier": "scenario_1"
}
response  = lambda_hanlder(test_event, None)


import React, { useState } from 'react';
import { useDropzone } from 'react-dropzone';
import { useNavigate } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowUpFromBracket, faTimes, faArrowLeft } from '@fortawesome/free-solid-svg-icons';
import styles from './Sidebar.module.css';
import FilePreviewModal from './FilePreviewModal';

const Sidebar = () => {
  const [file, setFile] = useState(null);
  const [showModal, setShowModal] = useState(false);
const navigate = useNavigate();

const handleBackClick = ()=>{
  navigate(-1);
}

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
          <p className={styles.fileName} onClick={openModal}>
            {file.name}
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



