
import React, { useEffect, useState } from 'react';
import axios from 'axios';
import { PropagateLoader } from 'react-spinners';
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
            {resume_identifier: "scenario_2"},
            {
              headers: {
                'Content-Type': 'application/json',
              }
            }
          );
          
          console.log('API Response:', response.data)

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
        <PropagateLoader color="rgb(15, 95, 220)" loading={loading} size={30} />
      </div>
    );
  }

  return (
    <div className={styles.mainContent}>
      {/* Job URL Section */}
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

      {/* Resume Summary Section */}
      <section className={styles.section}>
        <h3>Resume Summary</h3>
        <p>{data.resume_summary || 'No resume summary available'}</p>
      </section>

      {/* Skills Section */}
      <section className={styles.section}>
        <h3>Skills</h3>
        <p>{data.skills || 'No skills information available'}</p>
      </section>

      {/* Skill Suggestions Section */}
      <section className={styles.section}>
        <h3>Skill Suggestions</h3>
        <p>{data.skillsuggestion || 'No skill suggestions available'}</p>
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






import React, { useState } from 'react';
import { useDropzone } from 'react-dropzone';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowUpFromBracket, faTimes, faArrowLeft } from '@fortawesome/free-solid-svg-icons';
import styles from './Sidebar.module.css';
import FilePreviewModal from './FilePreviewModal';  // Make sure you create this file
import { useNavigate } from 'react-router-dom';

const Sidebar = ({ onFileUpload }) => {
  const [file, setFile] = useState(null);
  const [showModal, setShowModal] = useState(false);  // State to control modal visibility
  const navigate = useNavigate();

  // Function to handle file drop and set the file
  const onDrop = (acceptedFiles) => {
    setFile(acceptedFiles[0]);
    onFileUpload(); // Notify JobPage that a file has been uploaded
  };

  const { getRootProps, getInputProps } = useDropzone({ onDrop });

  // Function to remove the uploaded file
  const removeFile = () => {
    setFile(null);
  };

  // Function to open the preview modal
  const openModal = () => setShowModal(true);

  // Function to close the preview modal
  const closeModal = () => setShowModal(false);

  // Function to navigate back
  const handleBackClick = () => {
    navigate(-1); // Navigate back to the previous page
  };

  return (
    <div className={styles.sidebar}>
      <button className={styles.backButton} onClick={handleBackClick}>
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
          <p className={styles.fileName} onClick={openModal} role="button" tabIndex="0">
            {file.name}
          </p>
          <FontAwesomeIcon icon={faTimes} className={styles.removeIcon} onClick={removeFile} />
        </div>
      )}

      {/* Show the preview modal if the file is selected */}
      {showModal && file && (
        <FilePreviewModal
          file={file}
          closeModal={closeModal}
        />
      )}
    </div>
  );
};

export default Sidebar;
