import React, { useState } from 'react';
import axios from 'axios';
import styles from './Dashboard.module.css';
import Sidebar from './Sidebar';

import DashboardTable from './DashboardTable';

// Define file type mappings (keep this the same)
const FILE_TYPE_MAPPINGS = {
  'aimlusecasesv1': 'AI',
    'umo-privilegestatement': 'PS',
  'umo-invoice': 'CL',
  'umo-creditnote': 'MD',
  'umo-returnauthorisation': 'IN',
  'umo-accountstatement': 'OT'
};

function Dashboard({ userEmail }) {
  const [selectedCategory, setSelectedCategory] = useState('');
  const [selectedFile, setSelectedFile] = useState(null);
  const [uploadProgress, setUploadProgress] = useState(0);
  const [uploadStatus, setUploadStatus] = useState('');
  

  const handleFileChange = (file) => {
    if (file) {
      setSelectedFile(file);
    }
  };

  const handleUpload = async () => {
    // Keep the existing upload logic
    if (!selectedFile || !selectedCategory) {
      alert('Please select a file and category');
      return;
    }

    try {
      // Reset upload states
      setUploadProgress(0);
      setUploadStatus('');

      // Get file type code based on selected category
      const fileTypeCode = FILE_TYPE_MAPPINGS[selectedCategory] || 'OT';

      // Prepare payload for Lambda function
      const payload = {
        payload: {
          filename: selectedFile.name,
          filetype: fileTypeCode,
          fileSize: selectedFile.size,
          fileType: selectedFile.type
        }
      };

      // Rest of the upload logic remains the same...
      const urlResponse = await axios.post(
        'https://xk5c351fsc.execute-api.us-east-1.amazonaws.com/dev/file-uploading', 
        payload,
        {
          headers: {
            'Content-Type': 'application/json'
          }
        }
      );

      const { presignedUrl, key, recNum, bucket } = urlResponse.data;

      // Upload file to S3 using presigned URL
      const s3UploadResponse = await axios.put(presignedUrl, selectedFile, {
        headers: {
          'Content-Type': selectedFile.type
        },
        onUploadProgress: (progressEvent) => {
          const percentCompleted = Math.round(
            (progressEvent.loaded * 100) / progressEvent.total
          );
          setUploadProgress(percentCompleted);
        }
      });

      // Handle successful upload
      setUploadStatus('Upload successful!');
      
      console.log('Upload Details:', {
        recordNumber: recNum,
        s3Key: key,
        bucket: bucket
      });

      // Reset file selection after successful upload
      setSelectedFile(null);
      setSelectedCategory('');

    } catch (error) {
      console.error('Upload failed', error);
      
      if (error.response) {
        setUploadStatus(`Upload failed: ${error.response.data.message || 'Server error'}`);
      } else if (error.request) {
        setUploadStatus('No response from server');
      } else {
        setUploadStatus('Error preparing upload');
      }
    }
  };

  return (
    <div className={styles.dashboardLayout}>
      <Sidebar 
        selectedCategory={selectedCategory}
        setSelectedCategory={setSelectedCategory}
        selectedFile={selectedFile}
        handleFileChange={handleFileChange}
        handleUpload={handleUpload}
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


.dashboardLayout {
  display: flex;
  height: 100vh;
  background-color: #f4f6f9;
}



.recentUploadsList {
  list-style-type: none;
  padding: 0;
}

.recentUploadsList li {
  padding: 10px;
  border-bottom: 1px solid #e9ecef;
  color: #666;
  transition: background-color 0.3s ease;
}

.recentUploadsList li:hover {
  background-color: #f1f3f5;
}

.quickActions {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.quickActionBtn {
  padding: 10px;
  background-color: #6a11cb;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.quickActionBtn:hover {
  background-color: #2575fc;
}

.mainContent {
  flex-grow: 1;
  padding: 20px;
  /*overflow-y: auto;*/
  /*display: flex;*/
  justify-content: center;
  align-items: center;
  background-color: #f4f6f9;
}

/* Responsive Design */
@media (max-width: 768px) {
  .dashboardLayout {
    flex-direction: column;
  }

  .sidebar {
    width: 100%;
    height: auto;
  }

  .mainContent {
    height: 50vh;
  }
}


import React from 'react';
import styles from './Sidebar.module.css';
import DocumentCategorySelect from '../Category/DocumentCategorySelect';
import FileUploadSection from '../Upload/FileUploadSection';
import UploadProgressBar from '../Upload/UploadProgressBar';


function Sidebar({ 
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
    <div className={styles.sidebar}>
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

          <UploadProgressBar 
            progress={uploadProgress} 
            status={uploadStatus}
          />
        </div>
      </div>
    </div>
  );
}

export default Sidebar;



.sidebar {
  width: 350px;
  background-color: #ffffff;
  box-shadow: 2px 0 5px rgba(0, 0, 0, 0.05);
  /*overflow-y: auto;*/
  border-right: 1px solid #e9ecef;
}

.sidebar {
  width: 350px;
  background-color: #ffffff;
  box-shadow: 2px 0 5px rgba(0, 0, 0, 0.05);
  border-right: 1px solid #e9ecef;
  transition: transform 0.3s ease;
}

.sidebarClosed {
  transform: translateX(-100%);
  width: 0;
}

.sidebarToggle {
  position: fixed;
  left: 0;
  top: 50%;
  transform: translateY(-50%);
  background-color: #6a11cb;
  color: white;
  border: none;
  padding: 10px;
  z-index: 1000;
  cursor: pointer;
}

.sidebarContent {
  padding: 20px;
  /* Rest of your existing styles */
}


.sidebarContent {
  padding: 20px;
}

.sidebarTitle {
  font-size: 18px;
  color: #333;
  /*margin-bottom: 20px;*/
  border-bottom: 1.5px solid #6a11cb;
  padding-bottom: 10px;
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
