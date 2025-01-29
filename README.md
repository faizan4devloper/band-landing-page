import React, { useState } from 'react';
import styles from './Sidebar.module.css';
import DocumentCategorySelect from '../Category/DocumentCategorySelect';
import FileUploadSection from '../Upload/FileUploadSection';

import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronLeft, faChevronRight
} from '@fortawesome/free-solid-svg-icons';

function Sidebar({ 
  selectedCategory, 
  setSelectedCategory, 
  selectedFiles, 
  handleFileChange, 
  handleUpload, 
  uploadProgress, 
  uploadStatus,
  fileTypeMappings,
  fileInputRef,
  triggerFileInput
}) {
  const [isOpen, setIsOpen] = useState(true);

  const toggleSidebar = () => {
    setIsOpen(!isOpen);
  };

  return (
    <div className={`${styles.sidebarWrapper} ${isOpen ? styles.open : styles.closed}`}>
      <div className={styles.sidebar}>
        <div className={styles.sidebarContent}>
          <h3 className={styles.sidebarTitle}>Upload Document</h3>
          <div className={styles.sidebarSection}>
            <DocumentCategorySelect 
              selectedCategory={selectedCategory}
              onCategoryChange={setSelectedCategory}
              fileTypeMappings={fileTypeMappings}
            />
            
            <FileUploadSection 
              selectedFiles={selectedFiles}
              onFileChange={handleFileChange}
              onUpload={handleUpload}
              uploadProgress={uploadProgress}
              uploadComplete={uploadProgress === 100}
            />
          </div>
        </div>
      </div>
      
      <button 
        className={styles.toggleButton} 
        onClick={toggleSidebar}
      >
        {isOpen ? <FontAwesomeIcon icon={faChevronLeft} /> : <FontAwesomeIcon icon={faChevronRight}/>}
      </button>
    </div>
  );
}

export default Sidebar;



.sidebarWrapper {
  position: relative;
  display: flex;
  transition: all 0.3s ease;
}

.sidebar {
  width: 350px;
  background-color: #ffffff;
  box-shadow: 2px 0 5px rgba(0, 0, 0, 0.05);
  border-right: 1px solid #e9ecef;
  transition: all 0.3s ease;
  overflow-x: hidden;
}

.closed .sidebar {
  width: 0;
  opacity: 0;
}

.sidebarContent {
  padding: 20px;
  opacity: 1;
  transition: opacity 0.3s ease;
}

.closed .sidebarContent {
  opacity: 0;
  pointer-events: none;
}

.sidebarTitle {
  font-size: 16px;
  color: #fff;
  padding: 12px 20px;
  background-color: #6a11cb;
  border-radius: 4px;
}

.sidebarSection {
  margin-bottom: 30px;
  background-color: #f8f9fa;
  border-radius: 8px;
  padding: 15px;
}

.sidebarSection h3 {
  color: #6a11cb;
  margin-bottom: 15px;
  font-size: 1.1rem;
}

.toggleButton {
     position: absolute;
    top: 250px;
    left: 102%;
    transform: translateX(-50%);
    background-color: #6a11cb;
    color: white;
    border: none;
    border-radius: 0 4px 4px 0;
    padding: 10px 5px;
    cursor: pointer;
    z-index: 10;
    transition: background-color 0.3s ease;
}

.toggleButton:hover {
  background-color: #5a0faf;
}
