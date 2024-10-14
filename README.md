import React, { useState } from 'react';
import { useLocation } from 'react-router-dom';
import Sidebar from './Sidebar';
import DocumentPreview from './DocumentPreview'
import FormDisplay from './FormDisplay';
import styles from './UploadDocuments.module.css';
import data from '../../../Json/DocumentEntities.json'; // Import the JSON file

const UploadDocuments = () => {
    const location = useLocation();
    const { uploadedFile, documents = [] } = location.state || {};

    const allDocuments = [
        ...(uploadedFile ? [uploadedFile] : []),
        ...documents
    ];

    // No need to extract data here since FormDisplay uses the JSON directly.
    const [selectedForm, setSelectedForm] = useState(null);

    return (
        <div className={styles.container}>
            
            <div className={styles.uploadDocuments}>
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
            {/* Render the FormDisplay component, passing only the selectedForm */}
            <Sidebar onSelectForm={setSelectedForm} />
            
            {/* Render the Sidebar component, passing the setSelectedForm function */}
            <FormDisplay selectedForm={selectedForm} />
        </div>
    );
};

export default UploadDocuments;




.container {
  display: flex;
  flex-direction: row;
  align-items: flex-end;
  max-width: 100%;
  height: 100vh;
  padding: 20px;
  position: relative;
}
 
.uploadDocuments {
  flex: 2;
  background: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(10px);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);
  border-radius: 12px;
  padding: 20px;
  height: 100%;
  overflow-y: auto;
  transition: all 0.5s ease;
  display: none;
}
 
.previewVisible {
  display: block;
}
 
.documentHead {
  font-size: 1.8rem;
  font-weight: bold;
  margin-bottom: 20px;
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
 
/* Sidebar and FormDisplay shrinking effect */
.shrink {
  flex: 0.8;
  transition: all 0.5s ease;
}
 
.sidebar {
  flex: 1;
  max-width: 250px;
  height: 100%;
}
 
.formDisplay {
  flex: 3;
  height: 100%;
  max-width: 600px;
  overflow-y: auto;
}
 
/* Button to toggle document preview */
.togglePreviewButton {
  position: absolute;
  left: 0;
  top: 20px;
  background-color: #007bff;
  color: #fff;
  border: none;
  padding: 10px 20px;
  border-radius: 8px;
  cursor: pointer;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  transition: background-color 0.3s ease;
}
 
.togglePreviewButton:hover {
  background-color: #0056b3;
}
