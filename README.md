import React, { useState } from 'react';
import MainContent from './MainContent';
import Sidebar from './Sidebar';

const Dashboard = () => {
  const [file, setFile] = useState(null); // To track the uploaded file
  const [data, setData] = useState({
    joburl: '',
    resume_summary: '',
    skills: '',
    skillsuggestion: '',
    success_stories: {},
  });

  const handleFileUpload = (uploadedFile) => {
    setFile(uploadedFile);
    // Trigger the API call to fetch data based on the uploaded file
    fetchData(uploadedFile);
  };

  const handleFileRemove = () => {
    setFile(null);
    setData({
      joburl: '',
      resume_summary: '',
      skills: '',
      skillsuggestion: '',
      success_stories: {},
    });
  };

  const fetchData = async (uploadedFile) => {
    try {
      // Replace this with your actual API URL and logic
      const response = await axios.post('dummy', { resume_identifier: "scenario_1" }, {
        headers: {
          'Content-Type': 'application/json',
        }
      });

      let responseBody = response.data.body ? JSON.parse(response.data.body) : response.data;
      
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

  return (
    <div>
      <Sidebar onFileUpload={handleFileUpload} onFileRemove={handleFileRemove} />
      <MainContent file={file} data={data} />
    </div>
  );
};

export default Dashboard;



import React, { useState } from 'react';
import { useDropzone } from 'react-dropzone';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowUpFromBracket, faTimes, faArrowLeft } from '@fortawesome/free-solid-svg-icons';
import styles from './Sidebar.module.css';

const Sidebar = ({ onFileUpload, onFileRemove }) => {
  const [file, setFile] = useState(null);

  const onDrop = (acceptedFiles) => {
    const uploadedFile = acceptedFiles[0];
    setFile(uploadedFile);
    onFileUpload(uploadedFile); // Pass the uploaded file to the parent component
  };

  const { getRootProps, getInputProps } = useDropzone({ onDrop });

  const removeFile = () => {
    setFile(null);
    onFileRemove(); // Notify parent to clear data
  };

  return (
    <div className={styles.sidebar}>
      <button className={styles.backButton} aria-label="Go back">
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
          <p className={styles.fileName} role="button" tabIndex="0">
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
    </div>
  );
};

export default Sidebar;




import React from 'react';
import styles from './MainContent.module.css';

const MainContent = ({ file, data }) => {
  if (!file) {
    return <p>Please upload a resume to see the data.</p>;
  }

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
