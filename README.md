i want the after i click the faBars button in the NewClaimPage then open the sidebar and then hide the faBars and in the sidebar add close button after close then display the faBars button

import React, { useState } from "react";
import axios from "axios";
import Sidebar from "./Sidebar/Sidebar";
import MainContent from "./MainContent/MainContent";
import Verify from "./Verify/Verify";
import styles from "./NewClaimPage.module.css";
import BreadCrumbs from "./../BreadCrumbs/BreadCrumbs";
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faBars, faTimes } from '@fortawesome/free-solid-svg-icons';


  import { useClaimContext } from './../Context/ClaimContext';


const NewClaimPage = () => {
    const [isSidebarVisible, setIsSidebarVisible] = useState(true);
  
  const breadcrumbItems = [
    { label: 'Claims', link: '/claims' },
    { label: 'New Claim', link: '/new-claim' }
  ];

  const {
    file, setFile,
    message, setMessage,
    uploading, setUploading,
    uploadedFileName, setUploadedFileName,
    rows, setRows,
    isUploaded, setIsUploaded,
    previewUrl, setPreviewUrl,
    selectedPolicy, setSelectedPolicy
  } = useClaimContext();

  const handleFileChange = (e) => {
    const selectedFile = e.target.files[0];
    if (selectedFile) {
      const sanitizedFileName = selectedFile.name.replace(/\s+/g, "_");
      setFile(selectedFile);
      setUploadedFileName(sanitizedFileName);
      setPreviewUrl(URL.createObjectURL(selectedFile));
      setMessage("");
    }
  };

  const handlePolicyChange = (policy) => {
    setSelectedPolicy(policy);
  };
  
    const toggleSidebar = () => {
    setIsSidebarVisible(!isSidebarVisible);
  };


  const handleUpload = async () => {
    if (!file) {
      setMessage("Please select a file to upload.");
      return;
    }
    setUploading(true);
    try {
      const sanitizedFileName = file.name.replace(/\s+/g, "_");
      const response = await axios.post(
        "https:presignUrl",
        {
          payload: { filename: sanitizedFileName, filetype: "CL" },
        }
      );
      
      console.log("Presign url", response.data)

      const { presignedUrl, key, recNum } = response.data;
      
      console.log("**Response Api",response.data)

      await axios.put(presignedUrl, file, {
        headers: { "Content-Type": file.type },
      });

      const sqsPayload = {
        claimid: recNum,
        s3filename: key,
        // tasktype: "SEND_TO_QUEUE", 
  //       claimid : "CL004681",
  // s3filename: "claimassistv2/claimforms/CL004681/Case_CI_Cancer_3.1.pdf"
      };

    const sqsResponse=  await axios.post(
        "https:sqsqueue",
        sqsPayload,
        { headers: { "Content-Type": "application/json" } }
      );
      
      console.log("SQS API Response:", sqsResponse.data);

      setRows((prevRows) => [
        ...prevRows,
        {
          recNum,
          policyid: "",
          type: "",
          summary: "",
          previewLink: presignedUrl,
         policy: selectedPolicy, // Include the selected policy

          status: "Pending",
        },
      ]);

      setIsUploaded(true);
    } catch (error) {
      setMessage("Upload failed: " + error.message);
    } finally {
      setUploading(false);
    }
  };
  return (
    <div>
            <button 
        className={styles.toggleButton} 
        onClick={toggleSidebar}
      >
        <FontAwesomeIcon icon={isSidebarVisible ? faTimes : faBars} />
      </button>


<BreadCrumbs 
        items={breadcrumbItems} 
        // Optional: Add custom styling
        className={styles.customBreadcrumbs}
        style={{
          // Override default styles if needed
          backgroundColor: '#ffffff',
          borderBottom: '1px solid #e1e4e8'
        }}
      />

    <div className={styles.container}>
             <div className={`${styles.sidebar} ${!isSidebarVisible ? styles.sidebarHidden : ''}`}>
          <Sidebar
            onFileChange={handleFileChange}
            onUpload={handleUpload}
            uploading={uploading}
            onPolicyChange={handlePolicyChange}
            
          />
        </div>

      <div className={styles.mainContent}>
        {isUploaded ? (
          <MainContent 
            message={message} 
            selectedPolicy={selectedPolicy}
            rows={rows} 
            clid={rows[0].recNum}
            setRows={setRows} 
            staticPreviewUrl={previewUrl} 
            uploadedFileName={uploadedFileName}
          />
        ) : (
          <p className={styles.infoMessage}>
            Please upload a document to view the data.
          </p>
        )}
      </div>
    </div>
    </div>
  );
};

export default NewClaimPage;



.container {
  display: flex;
  height: 100vh;
  width: 100%;
  overflow: hidden;
}

.sidebar {
  flex: 0 0 300px;
  height: 100%;
  padding: 20px;
  box-sizing: border-box;
  transition: all 0.3s ease-in-out;
  transform: translateX(0);
}

.sidebarHidden {
  flex: 0 0 0;
  transform: translateX(-100%);
  opacity: 0;
  padding: 0;
  overflow: hidden;
}

.mainContent {
  flex: 1;
  height: 100%;
  background-color: #fff;
  overflow-y: auto;
  padding: 20px;
  box-sizing: border-box;
  /*transition: all 0.3s ease-in-out;*/
}

.toggleButton {
  position: fixed;
  top: 20px;
  left: 20px;
  z-index: 1000;
  background: linear-gradient(90deg, #0f5fdc, #7ca2e1);
  color: white;
  border: none;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  box-shadow: 0 2px 5px rgba(0,0,0,0.2);
  transition: transform 0.3s ease;
}

.toggleButton:hover {
  transform: scale(1.1);
}


import React, { useRef, useState } from "react";
import styles from "./Sidebar.module.css";
import { useNavigate } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowLeft, faTimes  } from '@fortawesome/free-solid-svg-icons';


const Sidebar = ({ onFileChange, onUpload, uploading, onPolicyChange , setFileNames }) => {
  const fileInputRef = useRef(null);
  const [fileName, setFileName] = useState("");
    const navigate = useNavigate();
    


  // Trigger the file input when the container is clicked
  const handleContainerClick = () => {
    if (fileInputRef.current) {
      fileInputRef.current.click();
    }
  };
  
  const handleBackClick = () => {
    navigate("/claims");
  };


  // Handle file selection and store the file name
  const handleFileChange = (event) => {
    const file = event.target.files[0];
    if (file) {
      
      const sanitizedFileName = file.name.replace(/\s+/g, '_');
      
      
      setFileName(sanitizedFileName);
      
      const renamedFile = new File([file], sanitizedFileName, { type:file.type });
      onFileChange({ target: { files: [renamedFile]}});
    }
    onFileChange(event); // Pass the file to the parent component
  };
  
  
   const handlePolicySelect = (e) => {
    onPolicyChange(e.target.value);
  };

  
  

  return (
    <div className={styles.sidebar}>
      
      <h2 className={styles.heading}>Manage Claim</h2>
      


      {/* File Input Container */}
      <div
        className={styles.fileInputContainer}
        onClick={handleContainerClick}
      >
        <p className={styles.dropzoneText}>Click to choose a file or drag & drop</p>
        <input
          type="file"
          ref={fileInputRef}
          onChange={handleFileChange}
          className={styles.fileInput}
          style={{ display: "none" }}
        />
      </div>

      {/* File Name Display */}
      {fileName && (
        <div className={styles.fileNameContainer}>
          <p className={styles.fileName}>{fileName}</p>
        </div>
      )}

      {/* Upload Button */}
      <button
        className={styles.uploadButton}
        onClick={onUpload}
        disabled={uploading}
      >
        {uploading ? (
          <div className={styles.loader}></div>
        ) : (
          "Upload"
        )}
      </button>
    </div>
  );
};

export default Sidebar;


/* Sidebar Container */
.sidebar {
  width: 250px;
  border-right: 1px solid rgba(0, 0, 0, 0.1);
  color: #333;
  padding: 20px;
  height: 100vh;
  overflow-y: hidden;
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* Heading */
.heading {
  font-size: 1.2rem;
  font-weight: bold;
  color: #0f5fdc;
  background: linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  margin-bottom: 20px;
}

/* Dropzone Container */
.dropzoneContainer {
  /*width: 100%;*/
  display: flex;
  justify-content: center;
  align-items: center;
  /*padding: 20px;*/
  border-radius: 10px;
  border: 2px dashed #ccc;
  background-color: #f9f9f9;
  transition: all 0.3s ease;
  cursor: pointer;
}

/* Dropzone Styling */
.dropzone {
  padding: 20px;
  text-align: center;
  color: #555;
  font-size: 14px;
  border-radius: 8px;
  width: 100%;
}

.dropzone:hover {
  background-color: #e8f5e9;
  border-color: #4caf50;
  color: #4caf50;
}

.dropzone p {
  margin: 0;
}

.backButton{
         position: absolute;
    top: 118px;
    left: 44px;
    padding: 8px 10px;
    background: linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
    color: white;
    font-size: 14px;
    border: none;
    border-radius: 4px;
    display: flex
;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: background-color 0.3s ease, transform 0.2s ease;
}

/* File Name Text */
.fileName {
  font-size: 14px;
  color: #333;
  font-weight: 600;
  margin: 0;
  word-wrap: break-word;
  text-overflow: ellipsis;
  overflow: hidden;
  white-space: nowrap;
}

/* File Input Container */


/* Upload Button */
.uploadButton {
  padding: 10px 20px;
  background: linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 20px;
}

.uploadButton:disabled {
  background: linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
  cursor: not-allowed;
}

.uploadButton:hover:not(:disabled) {
  background-color: #45a049;
}

/* Loader */
.loader {
  border: 3px solid #f3f3f3;
  border-top: 3px solid #4CAF50;
  border-radius: 50%;
  width: 16px;
  height: 16px;
  animation: spin 1s linear infinite;
}

/* Loader Animation */
@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

/* Upload Button Icon */
.icon {
  margin-right: 8px;
}

/* Message below file input */
.message {
  font-size: 12px;
  color: #555;
  margin-top: 10px;
}

/* Add visual feedback for drag events */
.dropzoneContainer.active {
  background-color: #e3f7e4;
  border-color: #4caf50;
}

.dropzoneContainer.reject {
  background-color: #fdecea;
  border-color: #e57373;
}

.errorMessage {
  color: #e57373;
  font-size: 12px;
  margin-top: 10px;
}

/* Styles for the file input container */
.fileInputContainer {
  border: 2px dashed #ccc;
  padding: 10px;
  border-radius: 10px;
  font-size: 1rem;
  text-align: center;
  position: relative;
  cursor: pointer;
}

.dragging {
  border-color: #00bcd4; /* Highlight when dragging over */
}

.browseContainer {
  margin-top: 10px;
}

.browseButton {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
  border-radius: 5px;
}

.browseButton:hover {
  background-color: #0056b3;
}

/* File Name Container */
.fileNameContainer {
  margin-top: 15px;
  /*background-color: #f3f3f3;*/
  border-radius: 8px;
  padding: 10px;
  width: 100%;
  text-align: center;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

/* File Name Text */
.fileName {
  font-size: 14px;
  color: #333;
  font-weight: 600;
  word-wrap: break-word;
  text-overflow: ellipsis;
  overflow: hidden;
  white-space: nowrap;
}

/* Dropdown select styling */
.policySelect {
  width: 100%;
  padding: 10px;
  font-size: 16px;
  border-radius: 5px;
  border: 1px solid #444;
  /*background-color: #333;*/
  /*color: white;*/
  margin-bottom: 20px;
  transition: background-color 0.3s ease;
}

.policySelect:hover,
.policySelect:focus {
  /*background-color: #444;*/
}
.closeSidebarButton {
    position: absolute;
    top: 10px;
    right: 10px;
    background: none;
    border: none;
    cursor: pointer;
    font-size: 24px;
    color: #333;
    z-index: 10;
}

