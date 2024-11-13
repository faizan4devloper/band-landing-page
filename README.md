{statusCode: 200, headers: {…}, body: '{"resume_summary": "Summary:\\n\\nJordan Michaels is…ts-Success-Story.pdf"}}}, "immigration_info": ""}'} this is output or response printed in console log but why not displaying in the UI


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
          'DUMMY',  // Replace with your actual API URL
          { resume_identifier: "scenario_1" },
          {
            headers: {
              'Content-Type': 'application/json',
            }
          }
        );
        console.log("API Response:", response.data); // Log response to verify structure

        // Ensure the response data is correctly formatted, defaulting to empty arrays if missing
        setData({
          techSkills: response.data.techSkills || [],
          jobPostings: response.data.jobPostings || [],
          similarProfiles: response.data.similarProfiles || [],
          immigrationInsights: response.data.immigrationInsights || [],
          crossSkilling: response.data.crossSkilling || [],
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
        <h3>Tech Skills, Certifications, Awards</h3>
        <ul>
          {data.techSkills?.map((item, index) => (
            <li key={index}>{item}</li>
          ))}
        </ul>
      </section>

      <section className={styles.section}>
        <h3>Top 5 Job Postings</h3>
        <ul>
          {data.jobPostings?.map((job, index) => (
            <li key={index}>
              {job.link ? (
                <a href={job.link} target="_blank" rel="noopener noreferrer">
                  {job.title}
                </a>
              ) : (
                <span>{job.title}</span>  // Fallback if no link
              )}
            </li>
          ))}
        </ul>
      </section>

      <section className={styles.section}>
        <h3>Similar Profiles - Job Recommendations</h3>
        <ul>
          {data.similarProfiles?.map((recommendation, index) => (
            <li key={index}>{recommendation}</li>
          ))}
        </ul>
      </section>

      <section className={styles.section}>
        <h3>Immigration and Visa Insights</h3>
        <ul>
          {data.immigrationInsights?.map((insight, index) => (
            <li key={index}>{insight}</li>
          ))}
        </ul>
      </section>

      <section className={styles.section}>
        <h3>Cross Skilling Recommendations</h3>
        <ul>
          {data.crossSkilling?.map((skill, index) => (
            <li key={index}>{skill}</li>
          ))}
        </ul>
      </section>
    </div>
  );
};

export default MainContent;




