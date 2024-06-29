import React from "react";
import "./JobMatching.css";

function JobMatching({ jobMatches }) {
  return (
    <div className="preview">
      <h3>Job Recommendations</h3>
      {jobMatches && <p>{jobMatches}</p>}
    </div>
  );
}

export default JobMatching;

.preview{
    background-color: rgba(0, 0, 0, 0.5);
    margin: 80px 0 0 10px;
    border-radius: 10px;
    width: 300px;
    padding: 20px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    transition: transform 0.3s ease-in-out;
}

.preview h3{
    color: #1f77f6
}
.preview p{
    padding: 12px;
    font-size: 12px;
    color:#fff;
}
