import React, { useState } from 'react';
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
  const [selectedFiles, setSelectedFiles] = useState([]);
  const [uploadProgress, setUploadProgress] = useState(0);
  const [uploadStatus, setUploadStatus] = useState('');

  // Handle file selection
  const handleFileChange = (files) => {
    if (files) {
      setSelectedFiles(files);
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
            'dummy', // Replace with actual API endpoint
            payload,
            { headers: { 'Content-Type': 'application/json' } }
          );

          const { presignedUrl, key, recNum, bucket } = urlResponse.data;

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

          uploadedFiles.push({ fileName: file.name, recordNumber: recNum, s3Key: key, bucket });

        } catch (error) {
          console.error(`Failed to upload file: ${file.name}`, error);
          failedFiles.push(file.name);
        }
      }

      if (failedFiles.length === 0) {
        setUploadStatus('All files uploaded successfully!');
      } else if (failedFiles.length === selectedFiles.length) {
        setUploadStatus('All uploads failed.');
      } else {
        setUploadStatus(
          `Partial success: ${uploadedFiles.length} files uploaded, ${failedFiles.length} failed.`
        );
      }

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
