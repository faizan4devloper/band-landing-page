import React, { useEffect, useState } from 'react';
import axios from 'axios';
import styles from './MainContent.module.css';

const MainContent = () => {
  const [data, setData] = useState({
    techSkills: [],
    jobPostings: [],
    similarProfiles: [],
    immigrationInsights: [],
    crossSkilling: [],
  });

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await axios.get('/api/data'); // Replace with your API endpoint
        setData(response.data);
      } catch (error) {
        console.error("Error fetching data:", error);
      }
    };
    fetchData();
  }, []);

  return (
    <div className={styles.mainContent}>
      <section className={styles.section}>
        <h3>Tech Skills, Certifications, Awards</h3>
        <ul>
          {data.techSkills.map((item, index) => (
            <li key={index}>{item}</li>
          ))}
        </ul>
      </section>
      <section className={styles.section}>
        <h3>Top 5 Job Postings</h3>
        <ul>
          {data.jobPostings.map((job, index) => (
            <li key={index}>
              <a href={job.link} target="_blank" rel="noopener noreferrer">
                {job.title}
              </a>
            </li>
          ))}
        </ul>
      </section>
      <section className={styles.section}>
        <h3>Similar Profiles - Job Recommendations</h3>
        <ul>
          {data.similarProfiles.map((recommendation, index) => (
            <li key={index}>{recommendation}</li>
          ))}
        </ul>
      </section>
      <section className={styles.section}>
        <h3>Immigration and Visa Insights</h3>
        <ul>
          {data.immigrationInsights.map((insight, index) => (
            <li key={index}>{insight}</li>
          ))}
        </ul>
      </section>
      <section className={styles.section}>
        <h3>Cross Skilling Recommendations</h3>
        <ul>
          {data.crossSkilling.map((skill, index) => (
            <li key={index}>{skill}</li>
          ))}
        </ul>
      </section>
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
  background-color: #f0f4f8;
}

.section {
  background-color: #ffffff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.section:hover {
  transform: translateY(-5px);
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
