import React, { useEffect, useState } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';

const MainContent = () => {
  const [data, setData] = useState({
    jobUrl: '',
    resumeSummary: '',
    skills: [],
    skillSuggestion: [],
    successStories: [],
  });

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await axios.post(
          'DUMMY',  // Replace with your actual API URL
          { resume_identifier: "scenario_1" },
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
          jobUrl: responseBody.jobUrl || '',
          resumeSummary: responseBody.resumeSummary || '',
          skills: responseBody.skills || [],
          skillSuggestion: responseBody.skillsuggestion || [],
          successStories: responseBody.success_stories || [],
        });
      } catch (error) {
        console.error("Error fetching data:", error);
      }
    };

    fetchData();
  }, []);

  return (
    <div className={styles.mainContent}>
      <section className={styles.section}>
        <h3>Job URL</h3>
        {data.jobUrl ? (
          <a href={data.jobUrl} target="_blank" rel="noopener noreferrer">{data.jobUrl}</a>
        ) : (
          <p>No job URL available</p>
        )}
      </section>

      <section className={styles.section}>
        <h3>Resume Summary</h3>
        {data.resumeSummary ? (
          <p>{data.resumeSummary}</p>
        ) : (
          <p>No resume summary available</p>
        )}
      </section>

      <section className={styles.section}>
        <h3>Skills</h3>
        <ul>
          {data.skills?.map((skill, index) => (
            <li key={index}>{skill}</li>
          ))}
        </ul>
      </section>

      <section className={styles.section}>
        <h3>Skill Suggestions</h3>
        <ul>
          {data.skillSuggestion?.map((suggestion, index) => (
            <li key={index}>{suggestion}</li>
          ))}
        </ul>
      </section>

      <section className={styles.section}>
        <h3>Success Stories</h3>
        <ul>
          {data.successStories?.map((story, index) => (
            <li key={index}>{story}</li>
          ))}
        </ul>
      </section>
    </div>
  );
};

export default MainContent;





joburl
resume_summary
skills
skillsuggestion
success_stories
