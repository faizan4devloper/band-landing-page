import React from "react";
import Skills from "./Skills";
import ChatWindow from "./ChatWindow";
import JobMatching from "./JobMatching";
// import "./JobAdvisor.css";

function JobAdvisor() {
  return (
    <div className="job-advisor">
      <ChatWindow />
      <Skills />
      <JobMatching />
    </div>
  );
}

export default JobAdvisor;
