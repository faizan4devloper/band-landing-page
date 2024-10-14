import React, { useState } from 'react';
import { useLocation } from 'react-router-dom';
import Sidebar from './Sidebar';
import DocumentPreview from './DocumentPreview';
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

    const [selectedForm, setSelectedForm] = useState(null);
    const [isSidebarOpen, setIsSidebarOpen] = useState(true); // Sidebar starts open by default

    // Function to handle the toggle state
    const handleToggleSidebar = () => {
        setIsSidebarOpen(!isSidebarOpen);
    };

    return (
        <div className={styles.container}>
            {/* Toggle button */}
            <button 
                className={styles.toggleSidebarButton} 
                onClick={handleToggleSidebar}
            >
                {isSidebarOpen ? 'Close Sidebar' : 'Open Sidebar'}
            </button>

            <div className={`${styles.sidebar} ${isSidebarOpen ? '' : styles.sidebarClosed}`}>
                <Sidebar onSelectForm={setSelectedForm} />
            </div>

            <div className={`${styles.formDisplay} ${isSidebarOpen ? '' : styles.formDisplayExpanded}`}>
                <FormDisplay selectedForm={selectedForm} />
            </div>

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
  max-width: 100%;
  height: 100vh;
  padding: 20px;
  position: relative;
  transition: all 0.5s ease;
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
}

.documentHead {
  font-size: 1.8rem;
  font-weight: bold;
  margin-bottom: 20px;
  color: #fff;
  text-align: center;
}

/* Flexbox shrinking and expanding effect */
.sidebar {
  flex: 0 0 250px; /* Sidebar initial width */
  transition: all 0.5s ease;
}

.sidebarClosed {
  flex: 0 0 50px; /* Shrink the sidebar to a smaller width when closed */
}

.formDisplay {
  flex: 3; /* Form display width when sidebar is open */
  transition: all 0.5s ease;
}

.formDisplayExpanded {
  flex: 515; /* Expand the form display when sidebar is closed */
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

/* Button to toggle sidebar */
.toggleSidebarButton {
  position: absolute;
  left: 20px;
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

.toggleSidebarButton:hover {
  background-color: #0056b3;
}


