import React, { useState } from 'react';
import Sidebar from './Sidebar';
import MainContent from './MainContent';

const JobPage = () => {
  const [isFileUploaded, setIsFileUploaded] = useState(false);

  // Function to handle file upload
  const handleFileUpload = () => {
    setIsFileUploaded(true);
  };

  // Function to handle file removal
  const handleFileRemove = () => {
    setIsFileUploaded(false);
  };

  return (
    <div style={{ display: 'flex' }}>
      <Sidebar onFileUpload={handleFileUpload} onFileRemove={handleFileRemove} />
      <MainContent isFileUploaded={isFileUploaded} clearData={!isFileUploaded} />
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

const MainContent = ({ isFileUploaded, clearData }) => {
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
      // Reset data if the clearData prop is true (file removed)
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
    if (isFileUploaded) {
      const fetchData = async () => {
        setLoading(true);

        try {
          const response = await axios.post(
            'DUMMY',  // Replace with your actual API URL
            { resume_identifier: "scenario_2" },
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
  }, [isFileUploaded]);

  if (loading) {
    return (
      <div className={styles.spinnerContainer}>
        <PropagateLoader color="rgb(15, 95, 220)" loading={loading} size={30} />
      </div>
    );
  }

  return (
    <div className={styles.mainContent}>
      <section className={styles.section}>
        <h3>Job URL</h3>
        {data.joburl ? (
          <ol className={styles.jobUrlList}>
            <li>
              <a href={data.joburl} target="_blank" rel="noopener noreferrer">
                <FontAwesomeIcon icon={faLink} className={styles.icon} /> View Job
              </a>
            </li>
          </ol>
        ) : (
          <p>No job URL available</p>
        )}
      </section>

      <section className={styles.section}>
        <h3>Resume Summary</h3>
        <p>{data.resume_summary || 'No resume summary available'}</p>
      </section>

      <section className={styles.section}>
        <h3>Skills</h3>
        <p>{data.skills || 'No skills information available'}</p>
      </section>

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
