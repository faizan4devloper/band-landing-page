import React, { useState } from 'react';
import Sidebar from './Sidebar';
import MainContent from './MainContent';
import axios from 'axios';

import styles from './JobPage.module.css';

const JobPage = () => {
  const [file, setFile] = useState(null);
  const [data, setData] = useState({
    joburl: '',
    resume_summary: '',
    skills: '',
    skillsuggestion: '',
    success_stories: {},
  });

  // Fetch data after file is uploaded
  const fetchData = async (uploadedFile) => {
    try {
      const formData = new FormData();
      formData.append('file', uploadedFile);

      // Replace with your actual API URL
      const response = await axios.post('dummy', formData, {
        headers: {
          'Content-Type': 'multipart/form-data',
        },
      });

      let responseBody;
      if (response.data.body) {
        responseBody = JSON.parse(response.data.body); // Parse the body if it's a string
      } else {
        responseBody = response.data;  // Fallback if body is not present
      }

      setData({
        joburl: responseBody.joburl || '',
        resume_summary: responseBody.resume_summary || '',
        skills: responseBody.skills || '',
        skillsuggestion: responseBody.skillsuggestion || '',
        success_stories: responseBody.success_stories || {},
      });
    } catch (error) {
      console.error('Error fetching data:', error);
    }
  };

  // Handle file upload
  const handleFileUpload = (acceptedFiles) => {
    const uploadedFile = acceptedFiles[0];
    setFile(uploadedFile);
    fetchData(uploadedFile);
  };

  // Handle file removal
  const handleFileRemoval = () => {
    setFile(null);
    setData({
      joburl: '',
      resume_summary: '',
      skills: '',
      skillsuggestion: '',
      success_stories: {},
    });
  };

  return (
    <div className={styles.jobPage}>
      <Sidebar onFileUpload={handleFileUpload} onFileRemoval={handleFileRemoval} file={file} />
      <MainContent data={data} />
    </div>
  );
};

export default JobPage;




import React, { useState } from 'react';
import { useDropzone } from 'react-dropzone';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowUpFromBracket, faTimes, faArrowLeft } from '@fortawesome/free-solid-svg-icons';
import styles from './Sidebar.module.css';
import { useNavigate } from 'react-router-dom';

const Sidebar = ({ onFileUpload, onFileRemoval, file }) => {
  const [showModal, setShowModal] = useState(false);
  const navigate = useNavigate();

  const handleBackClick = () => {
    navigate(-1);
  };

  const onDrop = (acceptedFiles) => {
    onFileUpload(acceptedFiles); // Call onFileUpload from JobPage
  };

  const { getRootProps, getInputProps } = useDropzone({ onDrop });

  const openModal = () => setShowModal(true);
  const closeModal = () => setShowModal(false);

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
          <p className={styles.fileName} role="button" tabIndex="0" onClick={openModal}>
            {file.name}
          </p>
          <FontAwesomeIcon
            icon={faTimes}
            className={styles.removeIcon}
            onClick={onFileRemoval} // Call onFileRemoval from JobPage
            aria-label="Remove file"
          />
        </div>
      )}

      {showModal && <FilePreviewModal file={file} closeModal={closeModal} />}
    </div>
  );
};

export default Sidebar;






import React from 'react';
import styles from './MainContent.module.css';

const MainContent = ({ data }) => {
  return (
    <div className={styles.mainContent}>
      <section className={styles.section}>
        <h3>Job URL</h3>
        {data.joburl ? (
          <a href={data.joburl} target="_blank" rel="noopener noreferrer">{data.joburl}</a>
        ) : (
          <p>No job URL available</p>
        )}
      </section>

      <section className={styles.section}>
        <h3>Resume Summary</h3>
        {data.resume_summary ? (
          <p>{data.resume_summary}</p>
        ) : (
          <p>No resume summary available</p>
        )}
      </section>

      <section className={styles.section}>
        <h3>Skills</h3>
        {data.skills ? (
          <p>{data.skills}</p>
        ) : (
          <p>No skills available</p>
        )}
      </section>

      <section className={styles.section}>
        <h3>Skill Suggestions</h3>
        {data.skillsuggestion ? (
          <p>{data.skillsuggestion}</p>
        ) : (
          <p>No skill suggestions available</p>
        )}
      </section>

      <section className={styles.section}>
        <h3>Success Stories</h3>
        {Object.keys(data.success_stories).length > 0 ? (
          <ul>
            {Object.keys(data.success_stories).map((storyKey, index) => (
              <li key={index}>
                <strong>{storyKey}:</strong> {JSON.stringify(data.success_stories[storyKey])}
              </li>
            ))}
          </ul>
        ) : (
          <p>No success stories available</p>
        )}
      </section>
    </div>
  );
};

export default MainContent;
