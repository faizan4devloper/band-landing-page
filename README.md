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
    const [isSidebarVisible, setIsSidebarVisible] = useState(true);

  const handleFileChange = (file) => {
    if (file) {
      setSelectedFile(file);
    }
  };
  
  
    const toggleSidebar = () => {
    setIsSidebarVisible(!isSidebarVisible);
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
        'dummy', 
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
            {isSidebarVisible && (
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
      )}



            <div className={`${styles.mainContent} ${!isSidebarVisible ? styles.fullWidth : ''}`}>
        {/* Add a toggle button for the sidebar */}
        <button 
          className={styles.sidebarToggleBtn} 
          onClick={toggleSidebar}
        >
          {isSidebarVisible ? '←' : '→'}
        </button>
        
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

.sidebarToggleBtn {
  position: fixed;
  top: 20px;
  left: 20px;
  z-index: 1000;
  background-color: #6a11cb;
  color: white;
  border: none;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.sidebarToggleBtn:hover {
  background-color: #2575fc;
}

.mainContent.fullWidth {
  width: 100%;
  flex-grow: 1;
}
