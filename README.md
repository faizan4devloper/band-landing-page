i want the multiple file can be uploaded and select from system folder:- import React, { useRef, useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { 
  faCloudUploadAlt,
  faPaperPlane,
  faFile, 
  faTimesCircle,
  faCheckCircle
} from '@fortawesome/free-solid-svg-icons';
import styles from './FileUploadSection.module.css';

function FileUploadSection({ 
  selectedFile, 
  onFileChange, 
  onUpload,
  allowedFileTypes = ['.pdf', '.doc', '.docx'],
  uploadProgress = 0,
  uploadComplete = false
}) {
  const [fileError, setFileError] = useState(null);
  const fileInputRef = useRef(null);

  const handleFileSelect = (event) => {
    const file = event.target.files[0];
    
    // Reset previous errors
    setFileError(null);

    // Validate file
    if (!file) {
      setFileError('No file selected');
      return;
    }

    // File size validation
    

    // File type validation
    const fileExtension = '.' + file.name.split('.').pop().toLowerCase();
    if (!allowedFileTypes.includes(fileExtension)) {
      setFileError(`Invalid file type. Allowed types: ${allowedFileTypes.join(', ')}`);
      return;
    }

    // Call file change handler
    onFileChange(file);
  };

  const handleUpload = () => {
    if (!selectedFile) {
      setFileError('Please select a file first');
      return;
    }
    onUpload();
  };

  const clearSelectedFile = () => {
    onFileChange(null);
    if (fileInputRef.current) {
      fileInputRef.current.value = '';
    }
  };

  return (
    <div className={styles.fileUploadContainer}>
      {/* File input and selection UI */}
      <div className={styles.fileUploadSection}>
        <input 
          type="file" 
          ref={fileInputRef}
          onChange={handleFileSelect}
          accept={allowedFileTypes.join(',')}
          style={{ display: 'none' }}
        />
        
        {/* File selection button */}
        <button 
          onClick={() => fileInputRef.current?.click()}
          className={styles.fileSelectBtn}
          disabled={uploadProgress > 0 && uploadProgress < 100}
        >
          <FontAwesomeIcon icon={faCloudUploadAlt} />
          Select File
        </button>
        
        {/* Selected file display */}
        {selectedFile && (
          <div className={styles.selectedFileInfo}>
            <FontAwesomeIcon icon={faFile} />
            <div>
              <span>{selectedFile.name}</span>
              <span>{(selectedFile.size / 1024).toFixed(2)} KB</span>
            </div>
            <button 
              onClick={clearSelectedFile}
              disabled={uploadProgress > 0 && uploadProgress < 100}
            >
              <FontAwesomeIcon icon={faTimesCircle} />
            </button>
          </div>
        )}

        {/* Progress bar */}
        {uploadProgress > 0 && uploadProgress < 100 && (
          <div className={styles.progressBarContainer}>
            <div 
              className={styles.progressBar} 
              style={{ width: `${uploadProgress}%` }}
            >
              {uploadProgress}%
            </div>
          </div>
        )}

        {/* Upload button */}
        <button 
          onClick={handleUpload}
          disabled={!selectedFile || (uploadProgress > 0 && uploadProgress < 100)}
        >
          {uploadComplete ? (
            <>
              <FontAwesomeIcon icon={faCheckCircle} />
              Upload Complete
            </>
          ) : (
            <>
              <FontAwesomeIcon icon={faPaperPlane} />
              Upload
            </>
          )}
        </button>
      </div>

      {/* Error message display */}
      {fileError && (
        <div className={styles.fileErrorMessage}>
          <FontAwesomeIcon icon={faTimesCircle} />
          {fileError}
        </div>
      )}
    </div>
  );
}

export default FileUploadSection;


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
