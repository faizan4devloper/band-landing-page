
import React, { useState, useRef } from 'react';
import axios from 'axios';
import styles from './Dashboard.module.css';
import Sidebar from './Sidebar';
import DashboardTable from './DashboardTable';
import Summary from '../SummaryContent/Summary';

// Define file type mappings
const FILE_TYPE_MAPPINGS = {
  aimlusecasesv1: 'AI',
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

  const handleFileChange = (fileList) => {
    if (!fileList) {
      setSelectedFiles(null);
      return;
    }
    setSelectedFiles(fileList.length > 0 ? Array.from(fileList) : null);
  };

  const triggerFileInput = () => {
    if (fileInputRef.current) {
      fileInputRef.current.click();
    }
  };

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

          const urlResponse = await axios.post(
            'dummy', // Replace 'dummy' with your actual API endpoint
            payload,
            { headers: { 'Content-Type': 'application/json' } }
          );

          const { presignedUrl, key, recNum, bucket } = urlResponse.data;

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
            bucket,
          });
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
        <Summary />
        <DashboardTable userEmail={userEmail} />
      </div>
    </div>
  );
}

export default Dashboard;







import React, { useState, useEffect } from 'react';
import axios from 'axios';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faSync, faEye } from '@fortawesome/free-solid-svg-icons';
import { motion, AnimatePresence } from 'framer-motion';
import Modal from 'react-modal';
import styles from './DashboardTable.module.css';

Modal.setAppElement('#root');

const DashboardTable = ({ userEmail }) => {
  const [documents, setDocuments] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [selectedDocUrl, setSelectedDocUrl] = useState('');
  const [isModalOpen, setIsModalOpen] = useState(false);

  const API_ENDPOINT = 'dummy'; // Replace with the actual API endpoint

  const fetchDocuments = async () => {
    setLoading(true);
    setError(null);
    try {
      const response = await axios.post(API_ENDPOINT, {
        headers: { 'Content-Type': 'application/json' },
      });

      setDocuments(response.data.documents);
    } catch (err) {
      setError('Failed to fetch documents');
      setDocuments([]);
    } finally {
      setLoading(false);
    }
  };

  const handlePreview = async (filename) => {
    try {
      const response = await axios.post(
        'dummy-preview-api', // Replace with your actual preview API endpoint
        { filename },
        { headers: { 'Content-Type': 'application/json' } }
      );

      const { presignedUrl } = response.data;
      setSelectedDocUrl(presignedUrl);
      setIsModalOpen(true);
    } catch (err) {
      console.error('Failed to fetch preview URL', err);
      alert('Unable to preview document.');
    }
  };

  const renderDocumentRow = (doc) => (
    <motion.tr
      key={doc.id}
      initial={{ opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
      transition={{ duration: 0.3 }}
      className={styles.documentRow}
    >
      <td>{doc.filename}</td>
      <td>{doc.category}</td>
      <td>
        <button
          className={styles.viewButton}
          onClick={() => handlePreview(doc.filename)}
        >
          <FontAwesomeIcon icon={faEye} /> Preview
        </button>
      </td>
    </motion.tr>
  );

  useEffect(() => {
    fetchDocuments();
  }, []);

  return (
    <div className={styles.documentTableContainer}>
      <table className={styles.documentTable}>
        <thead>
          <tr>
            <th>Filename</th>
            <th>Category</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <AnimatePresence>
            {loading
              ? 'Loading...'
              : documents.map((doc) => renderDocumentRow(doc))}
          </AnimatePresence>
        </tbody>
      </table>

      {/* Modal for preview */}
      <Modal
        isOpen={isModalOpen}
        onRequestClose={() => setIsModalOpen(false)}
        className={styles.modal}
        overlayClassName={styles.overlay}
      >
        <iframe src={selectedDocUrl} title="PDF Preview" className={styles.pdfViewer}></iframe>
        <button onClick={() => setIsModalOpen(false)}>Close</button>
      </Modal>
    </div>
  );
};

export default DashboardTable;
