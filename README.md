import React, { useState } from 'react';
import { useLocation } from 'react-router-dom';
import Sidebar from './Sidebar';
import DocumentPreview from './DocumentPreview';
import FormDisplay from './FormDisplay';
import styles from './UploadDocuments.module.css';

const UploadDocuments = () => {
    const location = useLocation();
    const { uploadedFile, documents = [] } = location.state || {};

    const allDocuments = [
        ...(uploadedFile ? [uploadedFile] : []),
        ...documents
    ];

    const [isPreviewVisible, setIsPreviewVisible] = useState(false); // State to manage toggle

    const togglePreview = () => {
        setIsPreviewVisible(!isPreviewVisible); // Toggle visibility
    };

    return (
        <div className={`${styles.container} ${isPreviewVisible ? styles.previewOpen : ''}`}>
            {/* Sidebar and FormDisplay sections */}
            <Sidebar className={styles.sidebar} />
            <FormDisplay className={styles.formDisplay} />

            {/* UploadDocuments section */}
            <div className={`${styles.uploadDocuments} ${isPreviewVisible ? styles.slideIn : styles.slideOut}`}>
                <h2 className={styles.documentHead}>Documents Review</h2>
                {allDocuments.length > 0 ? (
                    <div className={styles.reviewSection}>
                        <div className={styles.preview}>
                            {allDocuments.map((doc, index) => (
                                <DocumentPreview key={index} document={doc} />
                            ))}
                        </div>
                    </div>
                ) : (
                    <p className={styles.noFile}>No document available</p>
                )}
            </div>

            {/* Toggle Button */}
            <button className={styles.togglePreviewButton} onClick={togglePreview}>
                {isPreviewVisible ? 'Close Preview' : 'Open Preview'}
            </button>
        </div>
    );
};

export default UploadDocuments;



.container {
  display: flex;
  flex-direction: row;
  width: 100%;
  height: 100vh;
  transition: all 0.5s ease;
}

.sidebar {
  flex: 1; /* Initially taking full width */
  transition: all 0.5s ease;
}

.formDisplay {
  flex: 2; /* Initially taking full width */
  transition: all 0.5s ease;
  max-width: 600px;
  overflow-y: auto;
  background-color: #f9fafb;
  padding: 20px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  border-radius: 12px;
}

.uploadDocuments {
  position: absolute;
  left: -100%; /* Initially off-screen */
  top: 0;
  bottom: 0;
  width: 40%; /* Adjust this width as per your design */
  background: linear-gradient(135deg, #1e293b, #334155);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);
  backdrop-filter: blur(10px);
  border-radius: 12px;
  padding: 20px;
  height: 100%;
  overflow-y: auto;
  transition: all 0.5s ease; /* Smooth transition */
  opacity: 0;
}

.documentHead {
  font-size: 1.4rem;
  font-weight: bold;
  color: #fff;
  text-align: center;
}

.reviewSection {
  display: flex;
  flex-direction: column;
}

.noFile {
  color: #67748b;
  font-size: 1.2rem;
  text-align: center;
}

.preview {
  display: flex;
  flex-wrap: wrap;
  gap: 15px;
}

.preview > * {
  background-color: #e2e8f0;
  border-radius: 8px;
  padding: 10px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
}

.preview > *:hover {
  transform: scale(1.05);
}

/* Slide-in effect for the UploadDocuments component */
.previewOpen .uploadDocuments {
  left: 0; /* Bring into view */
  opacity: 1; /* Make visible */
}

.previewOpen .sidebar {
  flex: 0.5; /* Shrinks when the preview is visible */
}

.previewOpen .formDisplay {
  flex: 1; /* Shrinks when the preview is visible */
}

.togglePreviewButton {
  position: absolute;
  right: 50px;
  top: 30px;
  background: linear-gradient(135deg, #F2F2F2 -20%, #7ca2e1);
  color: #fff;
  border: none;
  padding: 8px 18px;
  border-radius: 6px;
  cursor: pointer;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  transition: background-color 0.3s ease, transform 0.3s ease;
}

.togglePreviewButton:hover {
  background-color: #0056b3;
}
