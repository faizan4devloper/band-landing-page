import React, { useEffect, useState } from 'react';
import axios from 'axios';
import { PropagateLoader } from 'react-spinners';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faLink } from "@fortawesome/free-solid-svg-icons";

// Utility function to convert URLs in text to anchor tags
const parseLinks = (text) => {
  const urlPattern = /(https?:\/\/[^\s]+)/g;
  return text.split(urlPattern).map((part, index) =>
    urlPattern.test(part) ? (
      <a key={index} href={part} target="_blank" rel="noopener noreferrer" className={styles.link}>
        {part}
      </a>
    ) : (
      part
    )
  );
};

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
            {Object.keys(data.success_stories).map((storyKey, index) => {
              const story = data.success_stories[storyKey];

              return (
                <li key={index}>
                  <h3>{story.title}</h3>
                  <p><strong>Outcome:</strong> {story.outcome}</p>
                  <p><strong>Transition:</strong> {story.transition}</p>
                  <p><strong>Skills:</strong> {story.skills.join(', ')}</p>
                  {story.pdf_link ? (
                    <div className={styles.pdfLinkWrapper}>
                      <a
                        href={story.pdf_link}
                        target="_blank"
                        rel="noopener noreferrer"
                        className={styles.pdfLink}
                      >
                        <FontAwesomeIcon icon={faLink} /> Read PDF
                      </a>
                    </div>
                  ) : (
                    <p>No PDF link available</p>
                  )}
                </li>
              );
            })}
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
