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
    const [isUploadVisible, setIsUploadVisible] = useState(false); // State to manage UploadDocuments visibility

    const toggleUpload = () => {
        setIsUploadVisible(prevState => !prevState); // Toggle visibility
    };

    return (
        <div className={styles.container}>
            {/* Sidebar and FormDisplay sections */}
            <div className={`${styles.sidebar} ${isUploadVisible ? styles.shrink : ''}`}>
                <Sidebar onSelectForm={setSelectedForm} />
            </div>
            <div className={`${styles.formDisplay} ${isUploadVisible ? styles.shrink : ''}`}>
                <FormDisplay selectedForm={selectedForm} />
            </div>

            {/* UploadDocuments section */}
            <div className={`${styles.uploadDocuments} ${isUploadVisible ? styles.slideIn : styles.slideOut}`}>
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
            <button className={styles.togglePreviewButton} onClick={toggleUpload}>
                {isUploadVisible ? 'Close Preview' : 'Open Preview'}
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

.uploadDocuments {
  position: absolute; /* Change to absolute positioning */
  right: 0; /* Align to the right */
  top: 0;
  height: 100%; /* Full height */
  width: 400px; /* Fixed width for UploadDocuments */
  background: linear-gradient(135deg, #1e293b, #334155);
  backdrop-filter: blur(10px);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);
  border-radius: 12px;
  padding: 20px;
  transform: translateX(100%); /* Start off-screen */
  transition: transform 0.5s ease, opacity 0.5s ease; /* Smooth transition */
  opacity: 0; /* Start hidden */
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

.noFile {
  color: #67748b;
  font-size: 1.2rem;
  text-align: center;
}

.preview {
  display: flex;
  margin: auto;
  flex-wrap: wrap;
  gap: 15px;
}

.preview > * {
  background-color: #e2e8f0;
  border-radius: 8px;
  padding: 10px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

/* Shrinking effect for Sidebar and FormDisplay */
.sidebar {
  flex: 1; /* Default width */
  max-width: 250px;
  height: 100%;
  transition: flex 0.5s ease, max-width 0.5s ease; /* Smooth transition */
}

.formDisplay {
  flex: 3; /* Default width */
  height: 100%;
  max-width: 600px;
  overflow-y: auto;
  transition: flex 0.5s ease, max-width 0.5s ease; /* Smooth transition */
}

/* Shrink effect when UploadDocuments is open */
.shrink {
  flex: 0.7; /* Adjust as needed */
  max-width: 200px; /* Reduced max-width for sidebar */
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
    const [isUploadVisible, setIsUploadVisible] = useState(false); // State to manage UploadDocuments visibility

    const toggleUpload = () => {
        setIsUploadVisible(!isUploadVisible); // Toggle visibility
    };

    return (
        <div className={styles.container}>
            {/* Sidebar and FormDisplay sections */}
            <Sidebar onSelectForm={setSelectedForm} className={isUploadVisible ? styles.shrink : ''} />
            <FormDisplay selectedForm={selectedForm} className={isUploadVisible ? styles.shrink : ''} />

            {/* UploadDocuments section */}
            <div className={`${styles.uploadDocuments} ${isUploadVisible ? styles.slideIn : styles.slideOut}`}>
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
            <button className={styles.togglePreviewButton} onClick={toggleUpload}>
                {isUploadVisible ? 'Close Preview' : 'Open Preview'}
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

.uploadDocuments {
  flex: 2;
  background: linear-gradient(135deg, #1e293b, #334155);
  backdrop-filter: blur(10px);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);
  border-radius: 12px;
  padding: 20px;
  height: 100%;
  overflow-y: auto;
  position: absolute; /* Change to absolute positioning */
  right: 0;
  top: 0;
  width: 100%; /* Full width to cover the right side */
  transform: translateX(100%); /* Initially off-screen */
  transition: all 0.5s ease; /* Smooth transition */
  opacity: 0; /* Start hidden */
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

.noFile {
  color: #67748b;
  font-size: 1.2rem;
  text-align: center;
}

.preview {
  display: flex;
  margin: auto;
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

/* Shrinking effect for Sidebar and FormDisplay */
.shrink {
  flex: 0.8; /* Shrinks when the preview is visible */
  transition: all 0.5s ease;
}

.sidebar {
  flex: 1;
  max-width: 250px;
  height: 100%;
  transition: all 0.5s ease;
}

.formDisplay {
  flex: 3;
  height: 100%;
  max-width: 600px;
  overflow-y: auto;
  transition: all 0.5s ease;
}

/* Button to toggle document preview */
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
