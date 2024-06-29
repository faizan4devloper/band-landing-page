import React from "react";
import "./JobMatching.css";

function JobMatching({ jobMatches }) {
  return (
    <div className="preview">
      <h3>Job Recommendations</h3>
      {jobMatches && (
        <div className="job-list">
          {jobMatches.map((job, index) => (
            <p key={index}>
              <a href={job.link} className="job-link" target="_blank" rel="noopener noreferrer">
                {job.title}
              </a>
            </p>
          ))}
        </div>
      )}
    </div>
  );
}

export default JobMatching;






.preview {
  background-color: rgba(0, 0, 0, 0.5);
  margin: 80px 0 0 10px;
  border-radius: 10px;
  width: 300px;
  padding: 20px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  transition: transform 0.3s ease-in-out;
}

.preview h3 {
  color: #1f77f6;
}

.preview .job-list {
  margin-top: 10px;
}

.preview .job-list p {
  padding: 8px 0;
}

.preview .job-list .job-link {
  color: #fff;
  text-decoration: none;
  font-size: 14px;
}

.preview .job-list .job-link:hover {
  text-decoration: underline;
}
