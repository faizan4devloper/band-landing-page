
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
            'DUMMY',  // Replace with your actual API URL
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
