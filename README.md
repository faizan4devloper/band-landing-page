import React, { useEffect, useState } from 'react';
import axios from 'axios';
import { ClipLoader } from 'react-spinners';
import styles from './MainContent.module.css';

const MainContent = () => {
  const [data, setData] = useState({
    joburl: '',
    resume_summary: '',
    skills: '',
    skillsuggestion: '',
    success_stories: {},
  });

  const [loading, setLoading] = useState(true);

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await axios.post(
          'https://7dyehbe7de.execute-api.us-east-1.amazonaws.com/emp/cit-adv2',  // Replace with your actual API URL
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
          responseBody = response.data;  // Fallback if body is not present
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
        setLoading(false);
      }
    };

    fetchData();
  }, []);

  return (
    <div className={styles.mainContent}>
      {loading ? (
        <div className={styles.spinnerContainer}>
          <ClipLoader size={50} color="#0073e6" />
        </div>
      ) : (
        <>
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
        </>
      )}
    </div>
  );
};

export default MainContent;



.mainContent {
  flex: 1;
  padding: 20px;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
  overflow-y: auto;
}

.spinnerContainer {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f0f4f8;
}

.section {
  padding: 20px;
  font-size: 1rem;
  background-color: #ffffff;
  border-left: 4px solid #0073e6;
  border-radius: 10px;
  max-height: 300px;
  overflow-y: auto;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  transition: box-shadow 0.3s ease, transform 0.3s ease;
}

.section:hover {
  box-shadow: 0 6px 15px rgba(0, 0, 0, 0.1);
  transform: translateY(-5px);
}

h3 {
  font-size: 1.3rem;
  color: #0073e6;
  margin-bottom: 15px;
}

ul {
  padding-left: 20px;
  list-style-type: disc;
}

li {
  font-size: 1rem;
  color: #333;
  margin-bottom: 10px;
}

a {
  color: #0073e6;
  text-decoration: none;
  font-weight: bold;
  transition: color 0.3s ease;
}

a:hover {
  color: #005bb5;
}

p {
  color: #555;
  font-size: 1rem;
}

.skills, .skillsuggestion {
  background-color: #f0f4f8;
  padding: 10px;
  border-radius: 5px;
  color: #333;
}

.skills ul, .skillsuggestion ul {
  padding-left: 20px;
  list-style-type: circle;
}

.skills li, .skillsuggestion li {
  font-size: 0.95rem;
  color: #666;
  margin-bottom: 8px;
}
