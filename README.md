import React, { useEffect, useState } from 'react';
import axios from 'axios';
import Sidebar from './Sidebar';
import styles from './MainContent.module.css';

const MainContent = () => {
  const [data, setData] = useState({
    techSkills: [],
    jobPostings: [],
    similarProfiles: [],
    immigrationInsights: [],
    crossSkilling: [],
  });

  const fetchData = async () => {
    try {
      const response = await axios.post(
        'dummy', // Replace with actual API endpoint
        { resume_identifier: "scenario_1" },
        {
          headers: {
            'Content-Type': 'application/json',
          },
        }
      );
      console.log("API Response:", response.data);
      setData(response.data); // Update the data in state after fetching
    } catch (error) {
      console.error("Error fetching data:", error);
    }
  };

  // Trigger data fetch after document upload
  const handleFileUpload = () => {
    fetchData(); // Call fetchData when file is uploaded
  };

  return (
    <div className={styles.mainContent}>
      <Sidebar onFileUpload={handleFileUpload} />
      <section className={styles.section}>
        <h3>Tech Skills, Certifications, Awards</h3>
        <ul>
          {data.techSkills?.map((item, index) => (
            <li key={index}>{item}</li>
          ))}
        </ul>
      </section>
      <section className={styles.section}>
        <h3>Top 5 Job Postings</h3>
        <ul>
          {data.jobPostings?.map((job, index) => (
            <li key={index}>
              <a href={job.link} target="_blank" rel="noopener noreferrer">
                {job.title}
              </a>
            </li>
          ))}
        </ul>
      </section>
      <section className={styles.section}>
        <h3>Similar Profiles - Job Recommendations</h3>
        <ul>
          {data.similarProfiles?.map((recommendation, index) => (
            <li key={index}>{recommendation}</li>
          ))}
        </ul>
      </section>
      <section className={styles.section}>
        <h3>Immigration and Visa Insights</h3>
        <ul>
          {data.immigrationInsights?.map((insight, index) => (
            <li key={index}>{insight}</li>
          ))}
        </ul>
      </section>
      <section className={styles.section}>
        <h3>Cross Skilling Recommendations</h3>
        <ul>
          {data.crossSkilling?.map((skill, index) => (
            <li key={index}>{skill}</li>
          ))}
        </ul>
      </section>
    </div>
  );
};

export default MainContent;







import React, { useState } from 'react';
import { useDropzone } from 'react-dropzone';
import { useNavigate } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowUpFromBracket, faTimes, faArrowLeft } from '@fortawesome/free-solid-svg-icons';
import styles from './Sidebar.module.css';
import FilePreviewModal from './FilePreviewModal';

const Sidebar = ({ onFileUpload }) => {
  const [file, setFile] = useState(null);
  const [showModal, setShowModal] = useState(false);
  const navigate = useNavigate();

  const handleBackClick = () => {
    navigate(-1);
  };

  const onDrop = (acceptedFiles) => {
    setFile(acceptedFiles[0]);
    onFileUpload(); // Trigger the data fetch after file upload
  };

  const { getRootProps, getInputProps } = useDropzone({ onDrop });

  const openModal = () => setShowModal(true);
  const closeModal = () => setShowModal(false);
  const removeFile = () => {
    setFile(null);
    closeModal();
  };

  return (
    <div className={styles.sidebar}>
      <button className={styles.backButton} onClick={handleBackClick} aria-label="Go back">
        <FontAwesomeIcon icon={faArrowLeft} />
      </button>
      <h3 className={styles.sidebarHeader}>Document Uploaded</h3>
      <div {...getRootProps()} className={styles.dropzone}>
        <input {...getInputProps()} aria-label="File dropzone" />
        <p>Drag & Drop, or click to select files</p>
        <FontAwesomeIcon icon={faArrowUpFromBracket} className={styles.uploadIcon} aria-label="Upload icon" />
      </div>
      {file && (
        <div className={styles.fileContainer}>
          <p className={styles.fileName} onClick={openModal} role="button" tabIndex="0">
            {file.name}
          </p>
          <FontAwesomeIcon
            icon={faTimes}
            className={styles.removeIcon}
            onClick={removeFile}
            aria-label="Remove file"
          />
        </div>
      )}

      {showModal && <FilePreviewModal file={file} closeModal={closeModal} />}
    </div>
  );
};

export default Sidebar;
