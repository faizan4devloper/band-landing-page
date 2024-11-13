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

  // useEffect hook to fetch data after the component mounts
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
        
        // Debugging: Check the API response structure
        console.log("API Response:", response.data);

        // Safely update the state based on the API response
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

    // Call the fetch function when the component is mounted
    fetchData();
  }, []);  // Empty dependency array to ensure it runs only once on mount

  // Debugging: Check if the data state is correctly updated
  useEffect(() => {
    console.log("Updated Data:", data);
  }, [data]);

  return (
    <div className={styles.mainContent}>
      <section className={styles.section}>
        <h3>Tech Skills, Certifications, Awards</h3>
        <ul>
          {data.techSkills.length > 0 ? (
            data.techSkills.map((item, index) => (
              <li key={index}>{item}</li>
            ))
          ) : (
            <p>No tech skills available</p>  // Fallback message if data is empty
          )}
        </ul>
      </section>

      <section className={styles.section}>
        <h3>Top 5 Job Postings</h3>
        <ul>
          {data.jobPostings.length > 0 ? (
            data.jobPostings.map((job, index) => (
              <li key={index}>
                {job.link ? (
                  <a href={job.link} target="_blank" rel="noopener noreferrer">
                    {job.title}
                  </a>
                ) : (
                  <span>{job.title}</span>  // Fallback if no link
                )}
              </li>
            ))
          ) : (
            <p>No job postings available</p>  // Fallback message
          )}
        </ul>
      </section>

      <section className={styles.section}>
        <h3>Similar Profiles - Job Recommendations</h3>
        <ul>
          {data.similarProfiles.length > 0 ? (
            data.similarProfiles.map((recommendation, index) => (
              <li key={index}>{recommendation}</li>
            ))
          ) : (
            <p>No similar profiles available</p>  // Fallback message
          )}
        </ul>
      </section>

      <section className={styles.section}>
        <h3>Immigration and Visa Insights</h3>
        <ul>
          {data.immigrationInsights.length > 0 ? (
            data.immigrationInsights.map((insight, index) => (
              <li key={index}>{insight}</li>
            ))
          ) : (
            <p>No immigration insights available</p>  // Fallback message
          )}
        </ul>
      </section>

      <section className={styles.section}>
        <h3>Cross Skilling Recommendations</h3>
        <ul>
          {data.crossSkilling.length > 0 ? (
            data.crossSkilling.map((skill, index) => (
              <li key={index}>{skill}</li>
            ))
          ) : (
            <p>No cross-skilling recommendations available</p>  // Fallback message
          )}
        </ul>
      </section>
    </div>
  );
};

export default MainContent;
