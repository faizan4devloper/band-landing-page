.mainContent {
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
  max-width: 800px;
  margin: 0 auto;
}

.section {
  margin-bottom: 20px;
  padding: 15px;
  background: #fff;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.sectionHead {
  font-size: 1.2em;
  font-weight: bold;
  color: #333;
  margin-bottom: 10px;
}

.skillsList, .suggestedSkillsList {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.skillBadge, .suggestedSkillBadge {
  padding: 8px 12px;
  background-color: #e0f3ff;
  color: #0f5fdc;
  border-radius: 15px;
  font-size: 0.9em;
}

.jobUrlList li a {
  text-decoration: none;
  color: #0f5fdc;
  font-weight: bold;
  padding: 8px 12px;
  border: 1px solid #0f5fdc;
  border-radius: 8px;
  transition: background-color 0.3s, color 0.3s;
}

.jobUrlList li a:hover {
  background-color: #0f5fdc;
  color: #fff;
}

.resumeHighlights, .successStoriesList {
  margin-top: 10px;
}

.successStoryCard {
  background: #f1f8ff;
  padding: 10px;
  border-radius: 8px;
  margin-bottom: 10px;
  border-left: 4px solid #0f5fdc;
}

.icon {
  margin-right: 8px;
}












import React, { useEffect, useState } from 'react';
import axios from 'axios';
import { PropagateLoader } from 'react-spinners';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faLink } from "@fortawesome/free-solid-svg-icons";

const MainContent = ({ isFileUploaded, resumeIdentifier, clearData }) => {
  const [data, setData] = useState({
    job_postings: '',
    resume_highlights: '',
    existing_skills: '',
    suggested_skills: '',
    success_stories: {},
  });
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    if (clearData) {
      setData({
        job_postings: '',
        resume_highlights: '',
        existing_skills: '',
        suggested_skills: '',
        success_stories: {},
      });
    }
  }, [clearData]);

  useEffect(() => {
    if (isFileUploaded && resumeIdentifier) {
      const fetchData = async () => {
        setLoading(true);

        try {
          const response = await axios.post(
            'dummy',  // Replace with your actual API URL
            { resume_identifier: resumeIdentifier },
            {
              headers: {
                'Content-Type': 'application/json',
              }
            }
          );

          console.log('API Response:', response.data);

          const responseBody = response.data.body ? JSON.parse(response.data.body) : response.data;
          
          console.log(responseBody)

          setData({
            job_postings: responseBody.job_postings || '',
            resume_highlights: responseBody.resume_highlights || '',
            existing_skills: responseBody.existing_skills || '',
            suggested_skills: responseBody.suggested_skills || '',
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
  }, [isFileUploaded, resumeIdentifier]);

  if (loading) {
    return (
      <div className={styles.spinnerContainer}>
        <PropagateLoader color="rgb(15, 95, 220)" loading={loading} size={25} />
      </div>
    );
  }

  return (
    <div className={styles.mainContent}>
      <section className={styles.section}>
        <p className={styles.sectionHead}>Existing Skills</p>
        <div className={styles.skillsList}>
          {data.existing_skills ? (
            data.existing_skills.split(',').map((skill, index) => (
              <span key={index} className={styles.skillBadge}>{skill.trim()}</span>
            ))
          ) : (
            <p>No skills information available</p>
          )}
        </div>
      </section>
      
      <section className={styles.section}>
        <p className={styles.sectionHead}>Job Postings</p>
        {data.job_postings ? (
          <ol className={styles.jobUrlList}>
            <li>
              <a href={data.job_postings} target="_blank" rel="noopener noreferrer">
                <FontAwesomeIcon icon={faLink} className={styles.icon} /> View Job
              </a>
            </li>
          </ol>
        ) : (
          <p>No job URL available</p>
        )}
      </section>

      <section className={styles.section}>
        <p className={styles.sectionHead}>Resume Highlights</p>
        <div className={styles.resumeHighlights}>
          {data.resume_highlights || 'No resume summary available'}
        </div>
      </section>

      <section className={styles.section}>
        <p className={styles.sectionHead}>Success Stories</p>
        {Object.keys(data.success_stories).length > 0 ? (
          <ul className={styles.successStoriesList}>
            {Object.keys(data.success_stories).map((storyKey, index) => (
              <li key={index} className={styles.successStoryCard}>
                <strong>{storyKey}:</strong> {JSON.stringify(data.success_stories[storyKey])}
              </li>
            ))}
          </ul>
        ) : (
          <p>No success stories available</p>
        )}
      </section>

      <section className={styles.section}>
        <p className={styles.sectionHead}>Suggested Skills</p>
        <div className={styles.suggestedSkillsList}>
          {data.suggested_skills ? (
            data.suggested_skills.split(',').map((skill, index) => (
              <span key={index} className={styles.suggestedSkillBadge}>{skill.trim()}</span>
            ))
          ) : (
            <p>No skill suggestions available</p>
          )}
        </div>
      </section>
    </div>
  );
};

export default MainContent;
