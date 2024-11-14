metadata
: 
pdf_link: "https://ac-aws-buc.s3


// import React, { useEffect, useState } from 'react';
// import axios from 'axios';
// import { PropagateLoader } from 'react-spinners';
// import styles from './MainContent.module.css';
// import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
// import { faLink } from "@fortawesome/free-solid-svg-icons";

// const MainContent = ({ isFileUploaded, clearData }) => {
//   const [data, setData] = useState({
//     joburl: '',
//     resume_summary: '',
//     skills: '',
//     skillsuggestion: '',
//     success_stories: {},
//   });
//   const [loading, setLoading] = useState(false);

//   useEffect(() => {
//     if (clearData) {
//       // Reset data if the clearData prop is true (file removed)
//       setData({
//         joburl: '',
//         resume_summary: '',
//         skills: '',
//         skillsuggestion: '',
//         success_stories: {},
//       });
//     }
//   }, [clearData]);

//   useEffect(() => {
//     if (isFileUploaded) {
//       const fetchData = async () => {
//         setLoading(true);

//         try {
//           const response = await axios.post(
//             'https://7dyehbe7de.execute-api.us-east-1.amazonaws.com/emp/cit-adv2',  // Replace with your actual API URL
//             { resume_identifier: "scenario_2" },
//             {
//               headers: {
//                 'Content-Type': 'application/json',
//               }
//             }
//           );

//           console.log('API Response:', response.data);

//           const responseBody = response.data.body ? JSON.parse(response.data.body) : response.data;
          
//           console.log(responseBody);

//           setData({
//             joburl: responseBody.joburl || '',
//             resume_summary: responseBody.resume_summary || '',
//             skills: responseBody.skills || '',
//             skillsuggestion: responseBody.skillsuggestion || '',
//             success_stories: responseBody.success_stories || {},
//           });
//         } catch (error) {
//           console.error("Error fetching data:", error);
//         } finally {
//           setLoading(false);
//         }
//       };

//       fetchData();
//     }
//   }, [isFileUploaded]);

//   if (loading) {
//     return (
//       <div className={styles.spinnerContainer}>
//         <PropagateLoader color="rgb(15, 95, 220)" loading={loading} size={30} />
//       </div>
//     );
//   }

//   return (
//     <div className={styles.mainContent}>
//       <section className={styles.section}>
//         <p className={styles.sectionHead}>Job URL</p>
//         {data.joburl ? (
//           <ol className={styles.jobUrlList}>
//             <li>
//               <a href={data.joburl} target="_blank" rel="noopener noreferrer">
//                 <FontAwesomeIcon icon={faLink} className={styles.icon} /> View Job
//               </a>
//             </li>
//           </ol>
//         ) : (
//           <p>No job URL available</p>
//         )}
//       </section>

//       <section className={styles.section}>
//         <p className={styles.sectionHead}>Resume Summary</p>
//         <p>{data.resume_summary || 'No resume summary available'}</p>
//       </section>

//       <section className={styles.section}>
//         <p className={styles.sectionHead}>Skills</p>
//         <p>{data.skills || 'No skills information available'}</p>
//       </section>

//       <section className={styles.section}>
//         <p className={styles.sectionHead}>Skill Suggestions</p>
//         <p>{data.skillsuggestion || 'No skill suggestions available'}</p>
//       </section>

//       <section className={styles.section}>
//         <p className={styles.sectionHead}>Success Stories</p>
//         {Object.keys(data.success_stories).length > 0 ? (
//           <ul>
//             {Object.keys(data.success_stories).map((storyKey, index) => (
//               <li key={index}>
//                 <strong>{storyKey}:</strong> {JSON.stringify(data.success_stories[storyKey])}
//               </li>
//             ))}
//           </ul>
//         ) : (
//           <p>No success stories available</p>
//         )}
//       </section>
//     </div>
//   );
// };

// export default MainContent;














the pdf link is not displaying


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
        
{
  Object.keys(data.success_stories).length > 0 ? (
    <ul>
      {Object.keys(data.success_stories).map((storyKey, index) => {
        const story = data.success_stories[storyKey];

        return (
          <li key={index}>
            {/* Add Story number */}
            <p className={styles.storyNo}>{`Story ${index + 1}`}</p>
            <p><strong>Outcome:</strong> {story.outcome}</p>
            <p><strong>Transition:</strong> {story.transition}</p>
            <p><strong>Skills:</strong> {story.skills.join(', ')}</p>
            {story.pdf_link ? (
              <div className={styles.pdfLinkWrapper}>
                {/* Display the URL link */}
                <a
                  href={story.pdf_link}
                  target="_blank"
                  rel="noopener noreferrer"
                  className={styles.pdfLink}
                >
                  Read More
                </a>
              </div>
            ) : (
              <p>No URL link available</p>
            )}
          </li>
        );
      })}
    </ul>
  ) : (
    <p>No success stories available</p>
  )
}

      </section>

      <section className={styles.section}>
        <p className={styles.sectionHead}>Suggested Skills</p>
        <p>{data.suggested_skills || 'No skill suggestions available'}</p>
      </section>
    </div>
  );
};

export default MainContent;
