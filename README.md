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
            {/* Sidebar and FormDisplay sections */}
            <Sidebar 
                onSelectForm={setSelectedForm} 
                className={`
                    ${isPreviewVisible ? styles.shrinkSidebar : styles.sidebarNormal}
                `} 
            />
            <FormDisplay 
                selectedForm={selectedForm} 
                className={`
                    ${isPreviewVisible ? styles.shrinkFormDisplay : styles.formDisplayNormal}
                `} 
            />

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
  align-items: flex-start;
  max-width: 100%;
  height: 100vh;
  padding: 20px;
  position: relative;
  transition: all 0.5s ease;
}

/* Shrinking effect for Sidebar and FormDisplay */
.shrinkSidebar {
  width: 250px; /* Shrinks Sidebar when preview is open */
  transition: width 0.5s ease;
}

.shrinkFormDisplay {
  flex: 0.3; /* 30% width for FormDisplay when preview is open */
  transition: flex 0.5s ease;
}

.sidebarNormal {
  width: 330px; /* Normal width for Sidebar */
  transition: width 0.5s ease;
}

.formDisplayNormal {
  flex: 1; /* 100% width for FormDisplay */
  transition: flex 0.5s ease;
}

/* Document Preview Section */
.uploadDocuments {
  flex: 2;
  background: linear-gradient(135deg, #1e293b, #334155);
  backdrop-filter: blur(10px);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);
  border-radius: 12px;
  padding: 20px;
  height: 100%;
  overflow-y: auto;
  right: 0;
  top: 0;
  width: 100%;
  transform: translateX(100%);
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
