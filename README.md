import React, { useState, useRef } from 'react';
import axios from 'axios';
import styles from './Dashboard.module.css';
import Sidebar from './Sidebar';
import DashboardTable from './DashboardTable';

// Define file type mappings
const FILE_TYPE_MAPPINGS = {
  'aimlusecasesv1': 'AI',
  'umo-privilegestatement': 'PS',
  'umo-invoice': 'CL',
  'umo-creditnote': 'MD',
  'umo-returnauthorisation': 'IN',
  'umo-accountstatement': 'OT',
};

function Dashboard({ userEmail }) {
  const [selectedCategory, setSelectedCategory] = useState('');
  const [selectedFiles, setSelectedFiles] = useState(null);
  const [uploadProgress, setUploadProgress] = useState(0);
  const [uploadStatus, setUploadStatus] = useState('');
  const fileInputRef = useRef(null);

  // Handle file selection
  const handleFileChange = (fileList) => {
    // If fileList is null, reset selected files
    if (!fileList) {
      setSelectedFiles(null);
      return;
    }

    // Convert fileList to array if it's not null
    setSelectedFiles(fileList.length > 0 ? Array.from(fileList) : null);
  };

  // Trigger file input (optional, can be used if needed)
  const triggerFileInput = () => {
    if (fileInputRef.current) {
      fileInputRef.current.click();
    }
  };

  // Handle file upload
  const handleUpload = async () => {
    if (!selectedFiles || selectedFiles.length === 0 || !selectedCategory) {
      alert('Please select at least one file and a category.');
      return;
    }

    try {
      setUploadProgress(0);
      setUploadStatus('Uploading...');
      
      const uploadedFiles = [];
      const failedFiles = [];

      for (let file of selectedFiles) {
        try {
          const fileTypeCode = FILE_TYPE_MAPPINGS[selectedCategory] || 'OT';
          const payload = {
            payload: {
              filename: file.name,
              filetype: fileTypeCode,
              fileSize: file.size,
              fileType: file.type,
            },
          };

          // Step 1: Request presigned URL
          const urlResponse = await axios.post(
            'dummy',
            payload,
            { headers: { 'Content-Type': 'application/json' } }
          );
          
                    console.log('Full URL Response:', urlResponse);
          console.log('Presigned URL Response Data:', urlResponse.data);


          const { presignedUrl, key, recNum, bucket } = urlResponse.data;
          
          console.log(presignedUrl)

          // Step 2: Upload file to S3
          await axios.put(presignedUrl, file, {
            headers: { 'Content-Type': file.type },
            onUploadProgress: (progressEvent) => {
              const percentCompleted = Math.round(
                (progressEvent.loaded * 100) / progressEvent.total
              );
              setUploadProgress(percentCompleted);
            },
          });

          uploadedFiles.push({ 
            fileName: file.name, 
            recordNumber: recNum, 
            s3Key: key, 
            bucket 
          });

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

      // Reset state after upload
      setSelectedFiles(null);
      setSelectedCategory('');
      setUploadProgress(0);

    } catch (error) {
      console.error('Error during upload', error);
      setUploadStatus('Upload failed: An unexpected error occurred.');
    }
  };

  return (
    <div className={styles.dashboardLayout}>
      <Sidebar
        selectedCategory={selectedCategory}
        setSelectedCategory={setSelectedCategory}
        selectedFiles={selectedFiles}
        handleFileChange={handleFileChange}
        handleUpload={handleUpload}
        uploadProgress={uploadProgress}
        uploadStatus={uploadStatus}
        fileTypeMappings={FILE_TYPE_MAPPINGS}
        fileInputRef={fileInputRef}
        triggerFileInput={triggerFileInput}
      />

      <div className={styles.mainContent}>
        <DashboardTable userEmail={userEmail} />
      </div>
    </div>
  );
}

export default Dashboard;


import React, { useRef, useState } from 'react';
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
    const fileList = event.target.files;
    
    // Reset previous errors
    setFileError(null);

    // Validate if at least one file is selected
    if (fileList.length === 0) {
      setFileError('No file selected');
      return;
    }

        // Validate each selected file
    const validFiles = Array.from(fileList).filter(file => {
      const fileExtension = '.' + file.name.split('.').pop().toLowerCase();
      return allowedFileTypes.includes(fileExtension);
    });


    if (validFiles.length === 0) {
      setFileError(`Invalid file type. Allowed types: ${allowedFileTypes.join(', ')}`);
      return;
    }



    // Call file change handler
    onFileChange(validFiles.length > 0 ? validFiles : null);
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
          multiple
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


import React from 'react';
import styles from './Sidebar.module.css';
import DocumentCategorySelect from '../Category/DocumentCategorySelect';
import FileUploadSection from '../Upload/FileUploadSection';
import UploadProgressBar from '../Upload/UploadProgressBar';

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
            selectedFile={selectedFiles ? selectedFiles[0] : null}
            onFileChange={handleFileChange}
            onUpload={handleUpload}
            uploadProgress={uploadProgress}
            uploadComplete={uploadProgress === 100}
            fileInputRef={fileInputRef}
            triggerFileInput={triggerFileInput}
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








