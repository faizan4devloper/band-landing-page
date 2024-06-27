import React from "react";
import Uploading from "./Uploading";
import ChatWindow from "./ChatWindow";
import Preview from "./Preview";
// import "./JobAdvisor.css";

function JobAdvisor() {
  return (
    <div className="job-advisor">
      <ChatWindow />
      <Uploading />
      <Preview />
    </div>
  );
}

export default JobAdvisor;



import React, { useState, useRef } from "react";

import "./UploadDoc.css";

function UploadDoc() {
  const [file, setFile] = useState(null);
    const fileInputRef = useRef(null);


  const handleFileClick = () => {
    fileInputRef.current.click();
  };

  const handleFileChange = (event) => {
    const selectedFile = event.target.files[0];
    if (selectedFile) {
      setFile(selectedFile);
    }
  };

  const handleUpload = () => {
    if (file) {
      // Simulate file upload
      console.log("Uploading file:", file);
      // Reset file input
      setFile(null);
      fileInputRef.current.value = "";
      alert("File uploaded successfully!");
    }
  };
  return (
    <div className="uploading">
      <h4>Upload Documents</h4>
      <button onClick={handleFileClick} className="upload-button">
        Select File
      </button>
      <input
        type="file"
        ref={fileInputRef}
        style={{ display: "none" }}
        onChange={handleFileChange}
      />
      {file && (
        <div className="file-info">
          <p>{file.name}</p>
          
        </div>
      )}
    </div>
  );
}

export default UploadDoc;
