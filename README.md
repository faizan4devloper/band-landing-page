import React, { useState } from 'react';
import { useDropzone } from 'react-dropzone';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowUpFromBracket } from '@fortawesome/free-solid-svg-icons';
import styles from './Sidebar.module.css';

// Modal component for preview
const FilePreviewModal = ({ file, closeModal }) => {
  const [fileContent, setFileContent] = useState(null);

  const readFile = (file) => {
    const reader = new FileReader();

    // For text files, read the contents
    if (file.type.startsWith('text')) {
      reader.onload = () => {
        setFileContent(reader.result);
      };
      reader.readAsText(file);
    }
    // For image files, display the image preview
    else if (file.type.startsWith('image')) {
      setFileContent(URL.createObjectURL(file));
    }
    else {
      setFileContent('Unable to preview this file type');
    }
  };

  // Read the file content when the modal is opened
  React.useEffect(() => {
    if (file) {
      readFile(file);
    }
  }, [file]);

  return (
    <div className={styles.modalOverlay} onClick={closeModal}>
      <div className={styles.modalContent} onClick={(e) => e.stopPropagation()}>
        <h2>File Preview</h2>
        {/* Preview based on file type */}
        {file && file.type.startsWith('text') && (
          <pre>{fileContent}</pre>
        )}
        {file && file.type.startsWith('image') && (
          <img src={fileContent} alt="preview" className={styles.imagePreview} />
        )}
        {file && !file.type.startsWith('text') && !file.type.startsWith('image') && (
          <p>{fileContent}</p>
        )}
        <button onClick={closeModal} className={styles.closeButton}>Close</button>
      </div>
    </div>
  );
};

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

  return (
    <div className={styles.sidebar}>
      <h2 className={styles.sidebarHeader}>Document Uploaded</h2>
      <div {...getRootProps()} className={styles.dropzone}>
        <input {...getInputProps()} />
        <p>Drag & Drop, or click to select files</p>
        <FontAwesomeIcon icon={faArrowUpFromBracket} className={styles.uploadIcon} />
      </div>
      {file && (
        <p className={styles.fileName} onClick={openModal}>
          {file.name} {/* Clicking on file name will open modal */}
        </p>
      )}

      {/* Modal for preview */}
      {showModal && <FilePreviewModal file={file} closeModal={closeModal} />}
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
  width: 600px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

/* Image preview */
.imagePreview {
  width: 100%;
  max-width: 500px;
  height: auto;
  margin-top: 20px;
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

/* For text file content display */
pre {
  white-space: pre-wrap;
  word-wrap: break-word;
  text-align: left;
  background-color: #f4f4f4;
  padding: 10px;
  border-radius: 5px;
  margin-top: 20px;
  max-height: 400px;
  overflow-y: auto;
}
