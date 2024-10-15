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

    return (
        <div className={styles.container}>
            {/* Sidebar section */}
            <Sidebar onSelectForm={setSelectedForm} />

            {/* FormDisplay section */}
            <FormDisplay selectedForm={selectedForm} />

            {/* UploadDocuments section */}
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
  gap: 20px; /* Add space between columns */
}

.sidebar {
  flex: 1;
  max-width: 250px;
  height: 100%;
  background-color: #1e293b;
  color: #fff;
  padding: 20px;
}

.formDisplay {
  flex: 2;
  max-width: 600px;
  height: 100%;
  overflow-y: auto;
  background-color: #f9fafb;
  padding: 20px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  border-radius: 12px;
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
