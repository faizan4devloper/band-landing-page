import React, { useState } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faChevronDown, faChevronUp } from "@fortawesome/free-solid-svg-icons";
import "./Skills.css";

function Skills({ suggestions }) {
  const [expanded, setExpanded] = useState(false);

  const toggleExpand = () => {
    setExpanded(!expanded);
  };

  return (
    <div className="skills-section">
      <h3 className="skills-heading">Skill Improvement and Suggestions</h3>
      <div className="toggle-button" onClick={toggleExpand}>
        <span>{expanded ? "Hide Details" : "Show Details"}</span>
        <FontAwesomeIcon icon={expanded ? faChevronUp : faChevronDown} />
      </div>
      {expanded && suggestions && (
        <div className="skills-content">
          <p>{suggestions}</p>
        </div>
      )}
    </div>
  );
}

export default Skills;




.skills-section {
  margin-bottom: 20px;
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 10px;
  background-color: #f9f9f9;
}

.skills-heading {
  font-size: 1.5rem;
  margin-bottom: 10px;
  color: #333;
}

.toggle-button {
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 8px;
  background-color: #e0e0e0;
  border-radius: 4px;
  margin-bottom: 10px;
}

.toggle-button span {
  font-weight: bold;
}

.skills-content {
  padding: 10px;
  background-color: #fff;
  border-radius: 4px;
}




import React from "react";
import "./JobMatching.css";

function JobMatching({ jobMatches }) {
  return (
    <div className="job-matching-section">
      <h3 className="job-matching-heading">Job Recommendations</h3>
      <div className="job-matching-content">
        {jobMatches ? (
          <ul className="job-list">
            {jobMatches.map((job, index) => (
              <li key={index}>
                <a href={job.url} target="_blank" rel="noopener noreferrer">{job.title}</a>
              </li>
            ))}
          </ul>
        ) : (
          <p>No job recommendations found.</p>
        )}
      </div>
    </div>
  );
}

export default JobMatching;



.job-matching-section {
  margin-bottom: 20px;
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 10px;
  background-color: #f9f9f9;
}

.job-matching-heading {
  font-size: 1.5rem;
  margin-bottom: 10px;
  color: #333;
}

.job-matching-content {
  padding: 10px;
}

.job-list {
  list-style-type: none;
  padding: 0;
}

.job-list li {
  margin-bottom: 10px;
}

.job-list li a {
  color: #007bff;
  text-decoration: none;
}

.job-list li a:hover {
  text-decoration: underline;
}
