import React, { useState } from 'react';
import { useDropzone } from 'react-dropzone';
import { useNavigate } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowUpFromBracket, faTimes, faArrowLeft } from '@fortawesome/free-solid-svg-icons';
import styles from './Sidebar.module.css';
import FilePreviewModal from './FilePreviewModal';

const Sidebar = () => {
  const [file, setFile] = useState(null);
  const [showModal, setShowModal] = useState(false);
  const [uploading, setUploading] = useState(false);
  const [uploadMessage, setUploadMessage] = useState('');
  const navigate = useNavigate();

  const handleBackClick = () => {
    navigate(-1);
  };

  const onDrop = async (acceptedFiles) => {
    const selectedFile = acceptedFiles[0];
    setFile(selectedFile);
    await uploadFile(selectedFile);
  };

  const { getRootProps, getInputProps } = useDropzone({ onDrop });

  const openModal = () => {
    setShowModal(true);
  };

  const closeModal = () => {
    setShowModal(false);
  };

  const removeFile = () => {
    setFile(null);
    closeModal();
  };

  const uploadFile = async (file) => {
    setUploading(true);
    setUploadMessage('Requesting upload URL...');

    try {
      // Step 1: Request a signed URL from the API Gateway endpoint
      const response = await fetch(
        `https://your-api-gateway-url/dev/getSignedUrl?filename=${file.name}&contentType=${file.type}`
      );

      if (!response.ok) throw new Error('Failed to get signed URL');

      const { uploadURL } = await response.json();

      // Step 2: Upload the file to S3 using the signed URL
      setUploadMessage('Uploading to S3...');
      const uploadResponse = await fetch(uploadURL, {
        method: 'PUT',
        body: file,
        headers: {
          'Content-Type': file.type,
        },
      });

      if (!uploadResponse.ok) throw new Error('Failed to upload file');

      setUploadMessage('File uploaded successfully!');
    } catch (error) {
      console.error('Upload error:', error);
      setUploadMessage('Upload failed. Please try again.');
    } finally {
      setUploading(false);
    }
  };

  return (
    <div className={styles.sidebar}>
      <button className={styles.backButton} onClick={handleBackClick}>
        <FontAwesomeIcon icon={faArrowLeft} />
      </button>
      <h3 className={styles.sidebarHeader}>Document Upload</h3>
      <div {...getRootProps()} className={styles.dropzone}>
        <input {...getInputProps()} />
        <p>Drag & Drop, or click to select files</p>
        <FontAwesomeIcon icon={faArrowUpFromBracket} className={styles.uploadIcon} />
      </div>
      {file && (
        <div className={styles.fileContainer}>
          <p className={styles.fileName} onClick={openModal}>
            {file.name}
          </p>
          <FontAwesomeIcon icon={faTimes} className={styles.removeIcon} onClick={removeFile} />
        </div>
      )}

      {/* Display upload status */}
      {uploading && <p className={styles.uploadMessage}>{uploadMessage}</p>}

      {/* Modal for preview */}
      {showModal && <FilePreviewModal file={file} closeModal={closeModal} />}
    </div>
  );
};

export default Sidebar;
