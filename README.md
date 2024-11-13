import React, { useEffect, useState } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';

const MainContent = () => {
  const [data, setData] = useState({
    joburl: '',
    resume_summary: '',
    skills: '',
    skillsuggestion: '',
    success_stories: {},
  });

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await axios.post(
          'dummy',  // Replace with your actual API URL
          { resume_identifier: "scenario_1" },
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
      }
    };

    fetchData();
  }, []);

  return (
    <div className={styles.mainContent}>
      <section className={styles.section}>
        <h3>Job URL</h3>
        {data.joburl ? (
          <ol className={styles.jobUrlList}>
            <li>
              <a href={data.joburl} target="_blank" rel="noopener noreferrer">1</a>
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




/* MainContent.module.css */

.mainContent {
  flex: 1;
  padding: 20px;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
  overflow-y: auto;
}

.section {
  padding: 15px;
  font-size: 0.8rem;
  background-color: #f9f9f9;
  border-left: 2px solid #0073e6;
  border-radius: 6px;
  max-height: 200px;
  overflow-y: auto;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.section:hover {
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
}

h3 {
  margin-bottom: 15px;
  font-size: 1.2rem;
  color: #333;
}

ul {
  padding-left: 20px;
  list-style-type: disc;
}

li {
  font-size: 0.95rem;
  color: #666;
  margin-bottom: 8px;
}

a {
  color: #0073e6;
  text-decoration: none;
  transition: color 0.3s;
}

a:hover {
  color: #005bb5;
}

/* New styles for job URL list */
.jobUrlList {
  padding-left: 0;
  list-style-type: none;
  margin: 0;
}

.jobUrlList li {
  display: inline;
  margin-right: 8px;
}

.jobUrlList a {
  color: #0073e6;
  font-weight: bold;
  text-decoration: none;
}

.jobUrlList a:hover {
  color: #005bb5;
  text-decoration: underline;
}
