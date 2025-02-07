i dont want that categories in backend i created only one bucket in that multi folders after i upload they auto mapp and goes to folders

import React, { useState } from 'react';
import styles from './Sidebar.module.css';
import DocumentCategorySelect from '../Category/DocumentCategorySelect';
import FileUploadSection from '../Upload/FileUploadSection';

import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faAnglesLeft, faAnglesRight
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
        {isOpen ? <FontAwesomeIcon icon={faAnglesLeft} /> : <FontAwesomeIcon icon={faAnglesRight}/>}
      </button>
    </div>
  );
}

export default Sidebar;


// DocumentCategorySelect.jsx
import React from 'react';
import PropTypes from 'prop-types';
import styles from './DocumentCategorySelect.module.css';

function DocumentCategorySelect({ 
  selectedCategory, 
  onCategoryChange,
  fileTypeMappings 
}) {
  const defaultFileTypeMappings = {
    'aimlusecasesv1': 'AI',
    'umo-privilegestatement': 'PS',
    'umo-invoice': 'CL',
    'umo-creditnote': 'MD',
    'umo-returnauthorisation': 'IN',
    'umo-accountstatement': 'OT'
  };

  const mappingsToUse = fileTypeMappings || defaultFileTypeMappings;

  return (
    <div className={styles.documentCategorySelectContainer}>
      <label className={styles.categoryLabel}>
        Document Category
        <div className={styles.selectWrapper}>
          <select
            value={selectedCategory}
            onChange={(e) => onCategoryChange(e.target.value)}
            className={styles.categoryDropdown}
            required
          >
            <option value="">Select Document Category</option>
            {Object.entries(mappingsToUse).map(([category, code]) => (
              <option key={category} value={category}>
                <div className={styles.categoryOption}>
                  {category} ({code})
                </div>
              </option>
            ))}
          </select>
          <span className={styles.selectArrow}>▼</span>
        </div>
      </label>
    </div>
  );
}

DocumentCategorySelect.propTypes = {
  selectedCategory: PropTypes.string.isRequired,
  onCategoryChange: PropTypes.func.isRequired,
  fileTypeMappings: PropTypes.objectOf(PropTypes.string)
};

DocumentCategorySelect.defaultProps = {
  fileTypeMappings: {
    'aimlusecasesv1': 'AI',
    'umo-privilegestatement': 'PS',
    'umo-invoice': 'CL',
    'umo-creditnote': 'MD',
    'umo-returnauthorisation': 'IN',
    'umo-accountstatement': 'OT'
  }
};

export default DocumentCategorySelect;




  import React, { useState, useRef } from 'react';
  import axios from 'axios';
  import styles from './Dashboard.module.css';
  import Sidebar from './Sidebar';
  import DashboardTable from './DashboardTable';
  import Summary from '../SummaryContent/Summary';
  
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
  
          // Step 1: Request presigned URL for the current file
          const urlResponse = await axios.post(
            'https:/file-uploading', // Replace 'dummy' with your actual API endpoint
            payload,
            { headers: { 'Content-Type': 'application/json' } }
          );
          
          console.log('URL:', urlResponse)
  
          const { presignedUrl, key, recNum, bucket } = urlResponse.data;
  
          // Step 2: Upload the current file to S3 using the presigned URL
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
          <Summary/>
          <DashboardTable userEmail={userEmail} />
        </div>
      </div>
    );
  }
  
  export default Dashboard;
  
  
  
