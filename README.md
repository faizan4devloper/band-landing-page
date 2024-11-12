import React, { useState } from 'react';
import styles from './FilePreviewModal.module.css'

// Modal component for preview
const FilePreviewModal = ({ file, closeModal }) => {
  const [fileContent, setFileContent] = useState(null);

  const readFile = (file) => {
    const reader = new FileReader();

    if (file.type.startsWith('text')) {
      reader.onload = () => {
        setFileContent(reader.result);
      };
      reader.readAsText(file);
    } else if (file.type.startsWith('image')) {
      setFileContent(URL.createObjectURL(file));
    } else if (file.type === 'application/pdf') {
      setFileContent(URL.createObjectURL(file)); // Set URL for PDF preview
    } else {
      setFileContent('Unable to preview this file type');
    }
  };

  React.useEffect(() => {
    if (file) {
      readFile(file);
    }
  }, [file]);

  return (
    <div className={styles.modalOverlay} onClick={closeModal}>
      <div className={styles.modalContent} onClick={(e) => e.stopPropagation()}>
        <h2>File Preview</h2>

        {file && file.type.startsWith('text') && <pre>{fileContent}</pre>}
        {file && file.type.startsWith('image') && (
          <img src={fileContent} alt="preview" className={styles.imagePreview} />
        )}
        {file && file.type === 'application/pdf' && (
          <iframe
            src={fileContent}
            title="PDF Preview"
            className={styles.pdfPreview}
          />
        )}
        {file && !file.type.startsWith('text') && !file.type.startsWith('image') && file.type !== 'application/pdf' && (
          <p>{fileContent}</p>
        )}

        <button onClick={closeModal} className={styles.closeButton}>Close</button>
      </div>
    </div>
  );
};

export default FilePreviewModal;


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

.pdfPreview {
  width: 100%;
  height: 500px;
  border: none;
}
