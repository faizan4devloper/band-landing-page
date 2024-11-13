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
            'https://7dyehbe7de.execute-api.us-east-1.amazonaws.com/emp/cit-adv2',  // Replace with your actual API URL
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
        <p>{data.existing_skills || 'No skills information available'}</p>
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
        <p>{data.resume_highlights || 'No resume summary available'}</p>
      </section>

      


      <section className={styles.section}>
        <p className={styles.sectionHead}>Success Stories</p>
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
      <section className={styles.section}>
        <p className={styles.sectionHead}>Suggested Skills</p>
        <p>{data.suggested_skills || 'No skill suggestions available'}</p>
      </section>
    </div>
  );
};

export default MainContent;




/* Style the overall container */
.mainContent {
  flex: 1;
  padding: 20px;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
  overflow-y: auto;
  scrollbar-width: thin; /* For Firefox */
  scrollbar-color: #0073e6 #f1f1f1; /* For Firefox */
}

/* Style the section container */
.section {
  padding: 15px;
  font-size: 0.8rem;
  background-color: #f9f9f9;
  border-left: 2px solid #0073e6;
  border-radius: 6px;
  height: 280px;
  overflow-y: auto;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  scrollbar-width: thin; /* For Firefox */
  scrollbar-color: #0073e6 #f1f1f1; /* For Firefox */
}

/* Hover effect on section */
.section:hover {
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
}

.section h3 {
  margin-bottom: 15px;
  font-size: 1.2rem;
  color: #333;
}

.section ul {
  padding-left: 0;
  list-style-type: none;
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}

.sectionHead{
  font-size: 14px;
  font-weight: bold;
  color:#572ac2;
  text-align: center;
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

.skillItem:hover {
  background-color: #005bb5;
  transform: scale(1.05);
}

/* Custom Scrollbar for Webkit Browsers (Chrome, Safari) */
.section::-webkit-scrollbar {
  width: 8px;
}

.section::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: 10px;
    border-radius: 10px;

}

.section::-webkit-scrollbar-thumb {
  background-color: #0073e6;
  border-radius: 10px;
  border: 2px solid #f1f1f1;
    border-radius: 10px;

}

.section::-webkit-scrollbar-thumb:hover {
  background-color: #005bb5;
    border-radius: 10px;

}

/* For the mainContent scroll */
.mainContent::-webkit-scrollbar {
  width: 8px;
    border-radius: 10px;

}

.mainContent::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: 10px;
}

.mainContent::-webkit-scrollbar-thumb {
  background-color: #0073e6;
  border-radius: 10px;
  border: 2px solid #f1f1f1;
}

.mainContent::-webkit-scrollbar-thumb:hover {
  background-color: #005bb5;
  
}

/* For Firefox scrollbar styling */
.scrollbarCustom {
  scrollbar-width: thin;
    border-radius: 10px;

  scrollbar-color: #0073e6 #f1f1f1;
}



/* MainContent.module.css */

/* Container for the loader to center it */
.spinnerContainer {
  position: absolute;
  top: 55%;
  left: 60%;
  transform: translate(-50%, -50%); /* Centering the loader */
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh; /* Takes full viewport height */
  width: 100%;   /* Takes full width */
  z-index: 999; /* Ensures the loader is on top of other content */
}

/* You can adjust the size of the loader itself if needed */
.loaderWrapper {
  display: flex;
  justify-content: center;
  align-items: center;
}

.clipLoader {
  border-color: #36d7b7 !important;  /* Sets loader color */
}


/* Style for success story links */
.link {
  text-decoration: none;
  color: #0f5fdc;
  font-weight: bold;
  padding: 4px 8px;
  border: 1px solid #0f5fdc;
  border-radius: 6px;
  transition: background-color 0.3s, color 0.3s;
}

.link:hover {
  background-color: #0f5fdc;
  color: #fff;
}
