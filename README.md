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

    const [selectedForm, setSelectedForm] = useState(null);
    const [isPreviewVisible, setIsPreviewVisible] = useState(false);

    const togglePreview = () => {
        setIsPreviewVisible(!isPreviewVisible); // Toggle visibility of UploadDocuments
    };

    return (
        <div className={`${styles.container} ${isPreviewVisible ? styles.previewVisible : ''}`}>
            {/* Sidebar section */}
            <Sidebar onSelectForm={setSelectedForm} className={styles.sidebar} />

            {/* FormDisplay section */}
            <FormDisplay selectedForm={selectedForm} className={styles.formDisplay} />

            {/* UploadDocuments section */}
            <div className={`${styles.uploadDocuments} ${isPreviewVisible ? styles.slideIn : ''}`}>
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
  align-items: flex-start;
  width: 100%;
  height: 100vh;
  padding: 20px;
  position: relative;
  transition: all 0.5s ease;
}

.sidebar {
  flex: 1;
  max-width: 250px;
  height: 100%;
  background-color: #1e293b;
  color: #fff;
  padding: 20px;
  transition: all 0.5s ease; /* Smooth shrinking */
}

.formDisplay {
  flex: 3;
  max-width: 600px;
  height: 100%;
  background-color: #f9fafb;
  padding: 20px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  border-radius: 12px;
  overflow-y: auto;
  transition: all 0.5s ease; /* Smooth shrinking */
}

.uploadDocuments {
  flex: 0;
  background: linear-gradient(135deg, #1e293b, #334155);
  backdrop-filter: blur(10px);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);
  border-radius: 12px;
  padding: 20px;
  height: 100%;
  width: 0;
  overflow-y: auto;
  position: absolute;
  left: 0;
  top: 0;
  transform: translateX(-100%);
  opacity: 0;
  transition: all 0.5s ease;
}

.previewVisible .sidebar {
  flex: 0.5; /* Shrink Sidebar */
}

.previewVisible .formDisplay {
  flex: 2; /* Shrink FormDisplay */
}

.slideIn {
  width: 30%; /* Visible size of UploadDocuments */
  transform: translateX(0);
  opacity: 1;
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
}

.togglePreviewButton {
  position: absolute;
  top: 20px;
  left: 20px;
  background-color: #7ca2e1;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 8px;
  cursor: pointer;
}

.togglePreviewButton:hover {
  background-color: #0056b3;
}
