import React, { useState } from 'react';
import styles from './Sidebar.module.css';
import FileUploadSection from '../Upload/FileUploadSection';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faAnglesLeft, faAnglesRight } from '@fortawesome/free-solid-svg-icons';

function Sidebar({ 
  selectedFiles, 
  handleFileChange, 
  handleUpload, 
  uploadProgress, 
  uploadStatus,
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

      <button className={styles.toggleButton} onClick={toggleSidebar}>
        {isOpen ? <FontAwesomeIcon icon={faAnglesLeft} /> : <FontAwesomeIcon icon={faAnglesRight} />}
      </button>
    </div>
  );
}

export default Sidebar;



import React, { useState, useRef } from 'react';
import axios from 'axios';
import styles from './Dashboard.module.css';
import Sidebar from './Sidebar';
import DashboardTable from './DashboardTable';
import Summary from '../SummaryContent/Summary';

function Dashboard({ userEmail }) {
  const [selectedFiles, setSelectedFiles] = useState(null);
  const [uploadProgress, setUploadProgress] = useState(0);
  const [uploadStatus, setUploadStatus] = useState('');
  const fileInputRef = useRef(null);

  // Handle file selection
  const handleFileChange = (fileList) => {
    setSelectedFiles(fileList?.length > 0 ? Array.from(fileList) : null);
  };

  // Trigger file input
  const triggerFileInput = () => {
    if (fileInputRef.current) {
      fileInputRef.current.click();
    }
  };

  // Handle file upload
  const handleUpload = async () => {
    if (!selectedFiles || selectedFiles.length === 0) {
      alert('Please select at least one file.');
      return;
    }

    try {
      setUploadProgress(0);
      setUploadStatus('Uploading...');

      const uploadedFiles = [];
      const failedFiles = [];

      for (let file of selectedFiles) {
        try {
          const payload = {
            payload: {
              filename: file.name,
              fileSize: file.size,
              fileType: file.type,
            },
          };

          // Step 1: Request presigned URL from backend
          const urlResponse = await axios.post(
            'https:/file-uploading', // Replace with actual API endpoint
            payload,
            { headers: { 'Content-Type': 'application/json' } }
          );

          const { presignedUrl, key, recNum, bucket } = urlResponse.data;

          // Step 2: Upload file to S3 using presigned URL
          await axios.put(presignedUrl, file, {
            headers: { 'Content-Type': file.type },
            onUploadProgress: (progressEvent) => {
              const percentCompleted = Math.round(
                (progressEvent.loaded * 100) / progressEvent.total
              );
              setUploadProgress(percentCompleted);
            },
          });

          uploadedFiles.push({ fileName: file.name, recordNumber: recNum, s3Key: key, bucket });
        } catch (error) {
          console.error(`Failed to upload file: ${file.name}`, error);
          failedFiles.push(file.name);
        }
      }

      // Update upload status
      if (failedFiles.length === 0) {
        setUploadStatus('All files uploaded successfully!');
      } else if (failedFiles.length === selectedFiles.length) {
        setUploadStatus('All uploads failed.');
      } else {
        setUploadStatus(
          `Partial success: ${uploadedFiles.length} files uploaded, ${failedFiles.length} failed.`
        );
      }

      // Reset after upload
      setSelectedFiles(null);
      setUploadProgress(0);

    } catch (error) {
      console.error('Error during upload', error);
      setUploadStatus('Upload failed: An unexpected error occurred.');
    }
  };

  return (
    <div className={styles.dashboardLayout}>
      <Sidebar
        selectedFiles={selectedFiles}
        handleFileChange={handleFileChange}
        handleUpload={handleUpload}
        uploadProgress={uploadProgress}
        uploadStatus={uploadStatus}
        fileInputRef={fileInputRef}
        triggerFileInput={triggerFileInput}
      />

      <div className={styles.mainContent}>
        <Summary />
        <DashboardTable userEmail={userEmail} />
      </div>
    </div>
  );
}

export default Dashboard;
