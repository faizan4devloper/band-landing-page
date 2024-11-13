import React, { useState } from 'react';
import Sidebar from './Sidebar';
import MainContent from './MainContent';
import styles from './JobPage.module.css';

const JobPage = () => {
  const [isFileUploaded, setIsFileUploaded] = useState(false);
  const [resumeIdentifier, setResumeIdentifier] = useState("");

  // Function to handle file upload
  const handleFileUpload = (file) => {
    setIsFileUploaded(true);
    if (file.name.includes("DataEngineerDataScientistResume")) {
      setResumeIdentifier("scenario_1");
    } else if (file.name.includes("NonTechDataEngineerResume")) {
      setResumeIdentifier("scenario_2");
    }
  };

  // Function to handle file removal
  const handleFileRemove = () => {
    setIsFileUploaded(false);
    setResumeIdentifier(""); // Reset the identifier
  };

  return (
    <div className={styles.jobPage}>
      <Sidebar onFileUpload={handleFileUpload} onFileRemove={handleFileRemove} />
      <MainContent isFileUploaded={isFileUploaded} resumeIdentifier={resumeIdentifier} clearData={!isFileUploaded} />
    </div>
  );
};

export default JobPage;



import React, { useEffect, useState } from 'react';
import axios from 'axios';
import { PropagateLoader } from 'react-spinners';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faLink } from "@fortawesome/free-solid-svg-icons";

const MainContent = ({ isFileUploaded, resumeIdentifier, clearData }) => {
  const [data, setData] = useState({
    joburl: '',
    resume_summary: '',
    skills: '',
    skillsuggestion: '',
    success_stories: {},
  });
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    if (clearData) {
      setData({
        joburl: '',
        resume_summary: '',
        skills: '',
        skillsuggestion: '',
        success_stories: {},
      });
    }
  }, [clearData]);

  useEffect(() => {
    if (isFileUploaded && resumeIdentifier) {
      const fetchData = async () => {
        setLoading(true);

        try {
          const response = await axios.post(
            'dummy',  // Replace with your actual API URL
            { resume_identifier: resumeIdentifier },
            {
              headers: {
                'Content-Type': 'application/json',
              }
            }
          );

          console.log('API Response:', response.data);

          const responseBody = response.data.body ? JSON.parse(response.data.body) : response.data;

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
          setLoading(false);
        }
      };

      fetchData();
    }
  }, [isFileUploaded, resumeIdentifier]);

  if (loading) {
    return (
      <div className={styles.spinnerContainer}>
        <PropagateLoader color="rgb(15, 95, 220)" loading={loading} size={30} />
      </div>
    );
  }

  return (
    <div className={styles.mainContent}>
      {/* Display sections as before */}
    </div>
  );
};

export default MainContent;






import React, { useState } from 'react';
import { useDropzone } from 'react-dropzone';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowUpFromBracket, faTimes, faArrowLeft } from '@fortawesome/free-solid-svg-icons';
import styles from './Sidebar.module.css';
import FilePreviewModal from './FilePreviewModal';
import { useNavigate } from 'react-router-dom';

const Sidebar = ({ onFileUpload, onFileRemove }) => {
  const [file, setFile] = useState(null);
  const [showModal, setShowModal] = useState(false);
  const navigate = useNavigate();

  const onDrop = (acceptedFiles) => {
    const uploadedFile = acceptedFiles[0];
    setFile(uploadedFile);
    onFileUpload(uploadedFile); // Pass the uploaded file to JobPage
  };

  const { getRootProps, getInputProps } = useDropzone({ onDrop });

  const removeFile = () => {
    setFile(null);
    onFileRemove();
  };

  const openModal = () => setShowModal(true);
  const closeModal = () => setShowModal(false);
  const handleBackClick = () => {
    navigate(-1);
  };

  return (
    <div className={styles.sidebar}>
      {/* Sidebar structure remains the same */}
    </div>
  );
};

export default Sidebar;
