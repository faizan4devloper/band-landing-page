import React, { useState } from 'react';
import Sidebar from './Sidebar';
import MainContent from './MainContent';
import styles from './JobPage.module.css';

const JobPage = () => {
  const [isFileUploaded, setIsFileUploaded] = useState(false);

  // Handle file upload status change
  const handleFileUpload = () => {
    setIsFileUploaded(true);
  };

  return (
    <div className={styles.jobPage}>
      <Sidebar onFileUpload={handleFileUpload} />
      <MainContent isFileUploaded={isFileUploaded} />
    </div>
  );
};

export default JobPage;



import React, { useState } from 'react';
import { useDropzone } from 'react-dropzone';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowUpFromBracket, faTimes, faArrowLeft } from '@fortawesome/free-solid-svg-icons';
import styles from './Sidebar.module.css';

const Sidebar = ({ onFileUpload }) => {
  const [file, setFile] = useState(null);

  const onDrop = (acceptedFiles) => {
    setFile(acceptedFiles[0]);
    onFileUpload(); // Notify JobPage that a file has been uploaded
  };

  const { getRootProps, getInputProps } = useDropzone({ onDrop });

  const removeFile = () => {
    setFile(null);
  };

  return (
    <div className={styles.sidebar}>
      <button className={styles.backButton} onClick={() => window.history.back()}>
        <FontAwesomeIcon icon={faArrowLeft} />
      </button>
      <h3 className={styles.sidebarHeader}>Document Uploaded</h3>
      <div {...getRootProps()} className={styles.dropzone}>
        <input {...getInputProps()} />
        <p>Drag & Drop, or click to select files</p>
        <FontAwesomeIcon icon={faArrowUpFromBracket} className={styles.uploadIcon} />
      </div>
      {file && (
        <div className={styles.fileContainer}>
          <p className={styles.fileName}>{file.name}</p>
          <FontAwesomeIcon icon={faTimes} className={styles.removeIcon} onClick={removeFile} />
        </div>
      )}
    </div>
  );
};

export default Sidebar;



import React, { useEffect, useState } from 'react';
import axios from 'axios';
import { ClipLoader } from 'react-spinners';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faLink } from "@fortawesome/free-solid-svg-icons";

const MainContent = ({ isFileUploaded }) => {
  const [data, setData] = useState({
    joburl: '',
    resume_summary: '',
    skills: '',
    skillsuggestion: '',
    success_stories: {},
  });
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    if (isFileUploaded) {
      const fetchData = async () => {
        setLoading(true); // Set loading to true when data fetching starts

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

          let responseBody;
          if (response.data.body) {
            responseBody = JSON.parse(response.data.body); // Parse the body if it's a string
          } else {
            responseBody = response.data;
          }

          setData({
            joburl: responseBody.joburl || '',
            resume_summary: responseBody.resume_summary || '',
            skills: responseBody.skills || '',
            skillsuggestion: responseBody.skillsuggestion || '',
            success_stories: responseBody.success_stories || {},
          });
        } catch (error) {
          console.error("Error fetching data:", error);
        } finally {
          setLoading(false); // Set loading to false once the data is fetched
        }
      };

      fetchData();
    }
  }, [isFileUploaded]); // Re-fetch data only when a file is uploaded

  if (loading) {
    return (
      <div className={styles.spinnerContainer}>
        <ClipLoader color="#36d7b7" loading={loading} size={50} />
      </div>
    );
  }

  return (
    <div className={styles.mainContent}>
      {/* Conditionally render sections */}
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

      {/* Other sections similarly */}
    </div>
  );
};

export default MainContent;
