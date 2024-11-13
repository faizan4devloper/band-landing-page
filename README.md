react-dom.development.js:13123 Uncaught Error: Objects are not valid as a React child (found: object with keys {transition, skills, outcome, metadata}). If you meant to render a collection of children, use an array instead.
    at throwOnInvalidObjectType (react-dom.development.js:13123:1)
    at createChild (react-dom.development.js:13375:1)
    at reconcileChildrenArray (react-dom.development.js:13640:1)
    at reconcileChildFibers (react-dom.development.js:14057:1)
    at reconcileChildren (react-dom.development.js:19186:1)
    at updateHostComponent (react-dom.development.js:19953:1)
    at beginWork (react-dom.development.js:21657:1)
    at HTMLUnknownElement.callCallback (react-dom.development.js:4164:1)
    at Object.invokeGuardedCallbackDev (react-dom.development.js:4213:1)
    at invokeGuardedCallback (react-dom.development.js:4277:1)





    
3
react-dom.development.js:13123 Uncaught Error: Objects are not valid as a React child (found: object with keys {transition, skills, outcome, metadata}). If you meant to render a collection of children, use an array instead.
    at throwOnInvalidObjectType (react-dom.development.js:13123:1)
    at createChild (react-dom.development.js:13375:1)
    at reconcileChildrenArray (react-dom.development.js:13640:1)
    at reconcileChildFibers (react-dom.development.js:14057:1)
    at reconcileChildren (react-dom.development.js:19186:1)
    at updateHostComponent (react-dom.development.js:19953:1)
    at beginWork (react-dom.development.js:21657:1)
    at HTMLUnknownElement.callCallback (react-dom.development.js:4164:1)
    at Object.invokeGuardedCallbackDev (react-dom.development.js:4213:1)
    at invokeGuardedCallback (react-dom.development.js:4277:1)
3
react-dom.development.js:18704 The above error occurred in the <li> component:

    at li
    at ul
    at section
    at div
    at MainContent (https://a6adf01….vfs.cloud9.us-east-1.amazonaws.com/main.03d4982….hot-update.js:165:3)
    at div
    at JobPage (https://a6adf01….vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:3209:94)
    at RenderedRoute (https://a6adf01….vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:46942:5)
    at Routes (https://a6adf01….vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:47644:5)
    at div
    at Router (https://a6adf01….vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:47578:15)
    at BrowserRouter (https://a6adf01….vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:45515:5)
    at AuthProvider (https://a6adf01….vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:4234:3)
    at Provider (https://a6adf01….vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:81552:3)
    at App (https://a6adf01….vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:54:84)
    at Provider (https://a6adf01….vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:81552:3)

Consider adding an error boundary to your tree to customize error handling behavior.
Visit https://reactjs.org/link/error-boundaries to learn more about error boundaries.
react-dom.development.js:13123 Uncaught Error: Objects are not valid as a React child (found: object with keys {transition, skills, outcome, metadata}). If you meant to render a collection of children, use an array instead.
    at throwOnInvalidObjectType (react-dom.development.js:13123:1)
    at createChild (react-dom.development.js:13375:1)
    at reconcileChildrenArray (react-dom.development.js:13640:1)
    at reconcileChildFibers (react-dom.development.js:14057:1)
    at reconcileChildren (react-dom.development.js:19186:1)
    at updateHostComponent (react-dom.development.js:19953:1)
    at beginWork (react-dom.development.js:21657:1)
    at beginWork$1 (react-dom.development.js:27465:1)
    at performUnitOfWork (react-dom.development.js:26596:1)
    at workLoopSync (react-dom.development.js:26505:1)
﻿


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
          
          console.log(responseBody);

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

  // Function to check if a string is a valid URL
  const isUrl = (str) => {
    const pattern = /^(https?:\/\/[^\s]+)$/;
    return pattern.test(str);
  };

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
              const storyValue = data.success_stories[storyKey];
              return (
                <li key={index}>
                  <strong>{storyKey}:</strong>{" "}
                  {isUrl(storyValue) ? (
                    // If it's a URL, make it clickable
                    <a href={storyValue} target="_blank" rel="noopener noreferrer" className={styles.link}>
                      {storyValue}
                    </a>
                  ) : (
                    // If it's not a URL, just display the text
                    storyValue
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

