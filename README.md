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
        const response = await axios.post(
          '/api/data',
          { resume_identifier: "scenario_1" },
          {
            headers: {
              'Content-Type': 'application/json',
              'Authorization': 'Bearer YOUR_ACCESS_TOKEN' // Replace with your token if needed
            }
          }
        );
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








This is Payload used : 
{
    "resume_identifier": "scenario_1"
}

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
        const response = await axios.post('/api/data'); // Replace with your API endpoint
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
