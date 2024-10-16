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
        setIsPreviewVisible(!isPreviewVisible);
    };

    return (
        <div className={styles.container}>
            {/* Sidebar with dynamic width */}
            <div
                style={{
                    width: isPreviewVisible ? '250px' : '330px',
                    transition: 'width 0.5s ease',
                }}
            >
                <Sidebar onSelectForm={setSelectedForm} />
            </div>

            {/* FormDisplay with dynamic flex */}
            <div
                style={{
                    flex: isPreviewVisible ? '0.3' : '1',
                    transition: 'flex 0.5s ease',
                }}
            >
                <FormDisplay selectedForm={selectedForm} />
            </div>

            {/* UploadDocuments section with slide effect */}
            <div
                className={`${styles.uploadDocuments} ${isPreviewVisible ? styles.slideIn : styles.slideOut}`}
            >
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
  width: 100%;
  height: 100vh;
  transition: all 0.5s ease;
}

/* UploadDocuments section */
.uploadDocuments {
  flex-grow: 1;
  background: linear-gradient(135deg, #1e293b, #334155);
  backdrop-filter: blur(10px);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);
  border-radius: 12px;
  padding: 20px;
  height: 100%;
  overflow-y: auto;
  transition: all 0.5s ease;
  opacity: 0;
}

/* Slide-in from the right */
.slideIn {
  transform: translateX(0);
  opacity: 1;
}

/* Slide-out to the right */
.slideOut {
  transform: translateX(100%);
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

.preview {
  display: flex;
  margin: auto;
  flex-wrap: wrap;
  gap: 15px;
}

.noFile {
  color: #67748b;
  font-size: 1.2rem;
  text-align: center;
}

/* Toggle Button */
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
