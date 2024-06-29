import React from "react";
import "./JobMatching.css";

function JobMatching({ jobMatches, setJobMatches }) {
  return (
    <div className="preview">
      <h3>Job Recommendations</h3>
      {jobMatches && <p>{jobMatches}</p>}
    </div>
  );
}

export default JobMatching;
