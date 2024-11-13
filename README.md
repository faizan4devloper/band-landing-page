import React, { useEffect, useState } from 'react';
import axios from 'axios';
import { ClipLoader } from 'react-spinners'; // Import the spinner
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faLink } from "@fortawesome/free-solid-svg-icons";

const MainContent = () => {
  const [data, setData] = useState({
    joburl: '',
    resume_summary: '',
    skills: '',
    skillsuggestion: '',
    success_stories: {},
  });
  const [loading, setLoading] = useState(true); // Track loading state

  useEffect(() => {
    const fetchData = async () => {
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
        console.log("API Response:", response.data); // Log response to verify structure

        // Assuming response.data.body contains the actual response as a string
        let responseBody;
        if (response.data.body) {
          responseBody = JSON.parse(response.data.body); // Parse the body if it's a string
        } else {
          responseBody = response.data;  // Fallback if body is not present
        }

        console.log("Parsed Response Body:", responseBody); // Log to verify the parsed response

        // Set state with the parsed data
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
  }, []);

  return (
    <div className={styles.mainContent}>
      {loading ? (
        <div className={styles.spinnerContainer}>
          <ClipLoader color="#36d7b7" loading={loading} size={50} />
        </div>
      ) : (
        <>
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
        </>
      )}
    </div>
  );
};

export default MainContent;
