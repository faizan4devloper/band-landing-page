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
  const [selectedFiles, setSelectedFiles] = useState([]);
  const [uploadProgress, setUploadProgress] = useState(0);
  const [uploadStatus, setUploadStatus] = useState('');
  

  const handleFileChange = (file) => {
    if (file) {
      setSelectedFiles(file);
    }
  };

const handleUpload = async () => {
  if (!selectedFiles || selectedFiles.length === 0 || !selectedCategory) {
    alert('Please select at least one file and a category');
    return;
  }

  try {
    // Reset upload states
    setUploadProgress(0);
    setUploadStatus('Uploading...');
    
    const uploadedFiles = []; // Store upload responses
    const failedFiles = [];   // Store failed files

    for (let file of selectedFiles) {
      try {
        // Get file type code based on selected category
        const fileTypeCode = FILE_TYPE_MAPPINGS[selectedCategory] || 'OT';

        // Prepare payload for Lambda function
        const payload = {
          payload: {
            filename: file.name,
            filetype: fileTypeCode,
            fileSize: file.size,
            fileType: file.type,
          },
        };

        // Step 1: Get presigned URL
        const urlResponse = await axios.post(
          'dummy', // Replace with actual API endpoint
          payload,
          {
            headers: {
              'Content-Type': 'application/json',
            },
          }
        );

        const { presignedUrl, key, recNum, bucket } = urlResponse.data;

        // Step 2: Upload file to S3 using presigned URL
        await axios.put(presignedUrl, file, {
          headers: {
            'Content-Type': file.type,
          },
          onUploadProgress: (progressEvent) => {
            const percentCompleted = Math.round(
              (progressEvent.loaded * 100) / progressEvent.total
            );
            setUploadProgress(percentCompleted);
          },
        });

        // Store successful upload details
        uploadedFiles.push({
          fileName: file.name,
          recordNumber: recNum,
          s3Key: key,
          bucket: bucket,
        });

      } catch (error) {
        console.error(`Failed to upload file: ${file.name}`, error);
        failedFiles.push(file.name);
      }
    }

    // Finalize upload status
    if (failedFiles.length === 0) {
      setUploadStatus('All files uploaded successfully!');
    } else if (failedFiles.length === selectedFiles.length) {
      setUploadStatus('All uploads failed.');
    } else {
      setUploadStatus(
        `Partial success: ${uploadedFiles.length} files uploaded, ${failedFiles.length} failed.`
      );
    }

    console.log('Uploaded Files:', uploadedFiles);
    console.log('Failed Files:', failedFiles);

    // Clear file selection after upload
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
        selectedFile={selectedFiles}
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
