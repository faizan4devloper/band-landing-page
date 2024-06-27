import React, { useState } from "react";
import UploadDoc from "./UploadDoc";
import ChatWindow from "./ChatWindow";
import Uploading from "./Uploading";
import Preview from "./Preview";
import "./JobAdvisor.css";

function JobAdvisor() {
  const [fileUploaded, setFileUploaded] = useState(false);

  const handleFileUploaded = () => {
    setFileUploaded(true);
  };

  return (
    <div className="job-advisor">
      <UploadDoc onFileUploaded={handleFileUploaded} />
      {fileUploaded && (
        <div className="additional-components show">
          <ChatWindow />
          <Uploading />
          <Preview />
        </div>
      )}
    </div>
  );
}

export default JobAdvisor;





import React, { useState, useRef } from "react";
import "./UploadDoc.css";

function UploadDoc({ onFileUploaded }) {
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
      // Notify parent component
      onFileUploaded();
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
          <button onClick={handleUpload} className="upload-button">
            Upload
          </button>
        </div>
      )}
    </div>
  );
}

export default UploadDoc;




/* JobAdvisor.css */
.job-advisor {
  text-align: center;
}

.upload-button {
  padding: 10px 20px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.2s;
  margin-top: 10px;
}

.upload-button:hover {
  background-color: #0056b3;
}

.upload-button:active {
  transform: scale(0.95);
}

.file-info {
  margin-top: 20px;
}

.file-info p {
  margin-bottom: 10px;
}

.additional-components {
  opacity: 0;
  height: 0;
  overflow: hidden;
  transition: opacity 0.5s ease, height 0.5s ease;
}

.additional-components.show {
  opacity: 1;
  height: auto;
}
