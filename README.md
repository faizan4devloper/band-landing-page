import React, { useState } from 'react';
import Sidebar from './Sidebar';
import MainContent from './MainContent';
import Chatbot from './ChatBot/Chatbot';

import styles from './JobPage.module.css';

const JobPage = () => {
  const [file, setFile] = useState(null);

  const handleFileUpload = (uploadedFile) => {
    setFile(uploadedFile);
  };

  return (
    <div className={styles.jobPage}>
      <Sidebar onFileUpload={handleFileUpload} />
      <MainContent file={file} />
    </div>
  );
};

export default JobPage;






import React, { useState } from 'react';
import { useDropzone } from 'react-dropzone';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowUpFromBracket, faTimes, faArrowLeft } from '@fortawesome/free-solid-svg-icons';
import styles from './Sidebar.module.css';
import FilePreviewModal from './FilePreviewModal';

const Sidebar = ({ onFileUpload }) => {
  const [file, setFile] = useState(null);
  const [showModal, setShowModal] = useState(false);

  const onDrop = (acceptedFiles) => {
    const uploadedFile = acceptedFiles[0];
    setFile(uploadedFile);
    onFileUpload(uploadedFile);  // Pass the file up to the parent component
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
      <button className={styles.backButton} onClick={() => window.history.back()} aria-label="Go back">
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







import React, { useEffect, useState } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faLink } from "@fortawesome/free-solid-svg-icons";

const MainContent = ({ file }) => {
  const [data, setData] = useState({
    joburl: '',
    resume_summary: '',
    skills: '',
    skillsuggestion: '',
    success_stories: {},
  });

  useEffect(() => {
    const fetchData = async () => {
      if (!file) return;  // Only fetch if a file is uploaded

      try {
        const response = await axios.post(
          'dummy',  // Replace with your actual API URL
          { resume_identifier: "scenario_2" },
          {
            headers: {
              'Content-Type': 'application/json',
            }
          }
        );
        console.log("API Response:", response.data);

        let responseBody;
        if (response.data.body) {
          responseBody = JSON.parse(response.data.body);
        } else {
          responseBody = response.data;
        }

        console.log("Parsed Response Body:", responseBody);

        setData({
          joburl: responseBody.joburl || '',
          resume_summary: responseBody.resume_summary || '',
          skills: responseBody.skills || '',
          skillsuggestion: responseBody.skillsuggestion || '',
          success_stories: responseBody.success_stories || {},
        });
      } catch (error) {
        console.error("Error fetching data:", error);
      }
    };

    fetchData();
  }, [file]); // Trigger fetch when file changes

  return (
    <div className={styles.mainContent}>
      <section className={styles.section}>
        <h3>Job URL</h3>
        {data.joburl ? (
          <ol className={styles.jobUrlList}>
            <li>
              <a href={data.joburl} target="_blank" rel="noopener noreferrer">
                <FontAwesomeIcon icon={faLink} className={styles.icon}/> View Job
              </a>
            </li>
          </ol>
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
