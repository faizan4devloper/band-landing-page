import React, { useState } from 'react';
import axios from 'axios';
import styles from './Dashboard.module.css';
import Sidebar from './Sidebar';
import DashboardTable from './DashboardTable';

const FILE_TYPE_MAPPINGS = {
  'aimlusecasesv1': 'AI',
  'umo-privilegestatement': 'PS',
  'umo-invoice': 'CL',
  'umo-creditnote': 'MD',
  'umo-returnauthorisation': 'IN',
  'umo-accountstatement': 'OT'
};

function Dashboard({ userEmail }) {
  const [sidebarOpen, setSidebarOpen] = useState(true); // Manage sidebar visibility
  const [selectedCategory, setSelectedCategory] = useState('');
  const [selectedFile, setSelectedFile] = useState(null);
  const [uploadProgress, setUploadProgress] = useState(0);
  const [uploadStatus, setUploadStatus] = useState('');

  const toggleSidebar = () => setSidebarOpen((prev) => !prev);

  // Rest of your file handling logic remains the same...

  return (
    <div className={styles.dashboardLayout}>
      {/* Sidebar with toggle functionality */}
      <button 
        className={styles.sidebarToggle} 
        onClick={toggleSidebar}
      >
        {sidebarOpen ? 'Close' : 'Open'}
      </button>

      <Sidebar
        isOpen={sidebarOpen}
        selectedCategory={selectedCategory}
        setSelectedCategory={setSelectedCategory}
        selectedFile={selectedFile}
        handleFileChange={(file) => setSelectedFile(file)}
        handleUpload={/* your upload logic */}
        uploadProgress={uploadProgress}
        uploadStatus={uploadStatus}
        fileTypeMappings={FILE_TYPE_MAPPINGS}
      />

      <div className={styles.mainContent}>
        <DashboardTable userEmail={userEmail} />
      </div>
    </div>
  );
}

export default Dashboard;





import React from 'react';
import styles from './Sidebar.module.css';
import DocumentCategorySelect from '../Category/DocumentCategorySelect';
import FileUploadSection from '../Upload/FileUploadSection';
import UploadProgressBar from '../Upload/UploadProgressBar';

function Sidebar({
  isOpen,
  selectedCategory,
  setSelectedCategory,
  selectedFile,
  handleFileChange,
  handleUpload,
  uploadProgress,
  uploadStatus,
  fileTypeMappings
}) {
  return (
    <div className={`${styles.sidebar} ${isOpen ? styles.sidebarOpen : styles.sidebarClosed}`}>
      <div className={styles.sidebarContent}>
        <h2 className={styles.sidebarTitle}>Dashboard</h2>
        <div className={styles.sidebarSection}>
          <h3>Upload Document</h3>
          <DocumentCategorySelect
            selectedCategory={selectedCategory}
            onCategoryChange={setSelectedCategory}
            fileTypeMappings={fileTypeMappings}
          />
          <FileUploadSection
            selectedFile={selectedFile}
            onFileChange={handleFileChange}
            onUpload={handleUpload}
          />
          <UploadProgressBar progress={uploadProgress} status={uploadStatus} />
        </div>
      </div>
    </div>
  );
}

export default Sidebar;




.sidebar {
  width: 350px;
  background-color: #ffffff;
  box-shadow: 2px 0 5px rgba(0, 0, 0, 0.1);
  border-right: 1px solid #e9ecef;
  transition: transform 0.3s ease-in-out;
  position: relative;
  z-index: 10;
}

.sidebarOpen {
  transform: translateX(0);
}

.sidebarClosed {
  transform: translateX(-100%);
}

.sidebarToggle {
  position: fixed;
  left: 10px;
  top: 50%;
  transform: translateY(-50%);
  background-color: #6a11cb;
  color: #fff;
  border: none;
  border-radius: 5px;
  padding: 10px 15px;
  cursor: pointer;
  z-index: 100;
  box-shadow: 2px 2px 8px rgba(0, 0, 0, 0.2);
  transition: background-color 0.3s ease;
}

.sidebarToggle:hover {
  background-color: #2575fc;
}
