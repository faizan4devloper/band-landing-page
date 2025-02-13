import React, { useRef, useState } from "react";
import styles from "./Sidebar.module.css";
import { useNavigate } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowLeft, faChevronLeft  } from '@fortawesome/free-solid-svg-icons';


const Sidebar = ({ onFileChange, onUpload, uploading, onPolicyChange , setFileNames, toggleSidebar }) => {
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
      <button 
                className={styles.closeSidebarButton} 
                onClick={toggleSidebar}
            >
                <FontAwesomeIcon icon={faChevronLeft} />
            </button>
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
    top: 53px;
    left: 40px;
    border-radius: 7%;
    background: linear-gradient(90deg, #0f5fdc, #7ca2e1);
    border: none;
    width: 35px;
    height: 31px;
    cursor: pointer;
    font-size: 15px;
    color: #fff;
    z-index: 10;
}


.sidebarHidden {
    width: 0;
    overflow: hidden;
}




import React, { useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { 
  faSync, 
  faFileAlt, 
  faChevronDown, 
  faChevronUp 
} from '@fortawesome/free-solid-svg-icons';
import styles from "./ExtractedContent.module.css";

const CollapsibleSection = ({ title, children }) => {
  const [isExpanded, setIsExpanded] = useState(false);

  return (
    <div className={styles.collapsibleSection}>
      <div 
        className={styles.sectionHeader} 
        onClick={() => setIsExpanded(!isExpanded)}
      >
        <h4>{title}</h4>
        <FontAwesomeIcon 
          icon={isExpanded ? faChevronUp : faChevronDown} 
          className={styles.toggleIcon}
        />
      </div>
      {isExpanded && (
        <div className={styles.sectionContent}>
          {children}
        </div>
      )}
    </div>
  );
};

const ExtractedContent = ({ data, loading, rows, handleReload }) => {
  const renderReadableContent = (data) => {
    if (!data) return (
      <div className={styles.noDataState}>
        <FontAwesomeIcon icon={faFileAlt} className={styles.noDataIcon} />
        <p className={styles.noDataText}>
          Processing the document. Please wait...
        </p>
      </div>
    );

    const parsedData = JSON.parse(data);
    console.log('Extraction ParsedData', parsedData)

    
    return (
      <div className={styles.readableContent}>
        {Object.keys(parsedData).map((section, index) => (
          <React.Fragment key={index}>
            {index > 0 && <div className={styles.sectionDivider}></div>}
            
            {section === 'CLAIM_FORM_DETAILS' && (
              <CollapsibleSection title="Claim Form Details">
                <p><strong>Type:</strong> {parsedData[section]?.CLAIM_FORM_TYPE || 'Not specified'}</p>
              </CollapsibleSection>
            )}

            {section === 'CLINICAL_ABSTRACT_APPLICATION' && (
              <CollapsibleSection title="Clinical Abstract Application">
                <p><strong>Name of Patient:</strong> {parsedData[section]?.NAME_OF_PATIENT || 'N/A'}</p>
                <p><strong>NRIC/FIN/BC:</strong> {parsedData[section]?.NRIC_FIN_BC || 'N/A'}</p>
                <p><strong>Address:</strong> {parsedData[section]?.ADDRESS || 'N/A'}</p>
                <p><strong>Date:</strong> {parsedData[section]?.DATE || 'N/A'}</p>
              </CollapsibleSection>
            )}

            {/* Similar pattern for other sections */}
            {section === 'CLAIMANT_STATEMENT' && (
              <CollapsibleSection title="Claimant Statement">
                {parsedData[section]?.POLICY_DETAILS && (
                  <div>
                    <h5>Policy Details</h5>
                    <p><strong>Policy Numbers:</strong> {parsedData[section].POLICY_DETAILS?.POLICY_NUMBERS || 'N/A'}</p>
                    <p><strong>Type of Claim:</strong> {parsedData[section].POLICY_DETAILS?.TYPE_OF_CLAIM || 'N/A'}</p>
                  </div>
                )}

                {parsedData[section]?.DETAILS_OF_LIFE_ASSURED && (
                  <div>
                    <h5>Details of Life Assured</h5>
                    <p><strong>Name:</strong> {parsedData[section].DETAILS_OF_LIFE_ASSURED?.NAME_OF_LIFE_ASSURED || 'N/A'}</p>
                    <p><strong>Gender:</strong> {parsedData[section].DETAILS_OF_LIFE_ASSURED?.GENDER || 'N/A'}</p>
                    <p><strong>Marital Status:</strong> {parsedData[section].DETAILS_OF_LIFE_ASSURED?.MARITAL_STATUS || 'N/A'}</p>
                  </div>
                )}
              </CollapsibleSection>
            )}
            
            {section === 'DETAILS_OF_ILLNESS' && (
              <CollapsibleSection title="Details of Illness">

            <h4></h4>
            {parsedData[section]?.DETAILS && (
              <>
                <p><strong>Date when patient first consulted:</strong> {parsedData[section].DETAILS?.DATE_PATIENT_FIRST_CONSULTED || 'N/A'}</p>
                <p><strong>Symptoms presented at first consultation:</strong> {parsedData[section].DETAILS?.DETAILS_OF_SYMPTOMS_PRESENTED_AT_FIRST_CONSULTATION || 'N/A'}</p>
                <p><strong>Underlying of Symptoms:</strong> {parsedData[section].DETAILS?.UNDERLYING_OF_SYMPTOMS || 'N/A'}</p>
                <p><strong>Exact Diagnosis of Condition:</strong> {parsedData[section].DETAILS?.EXACT_DIAGNOSIS_OF_CONDITION || 'N/A'}</p>
                
                {parsedData[section].DETAILS?.PRIMARY_AND_EXACT_SITE_OF_TUMOUR && (
                  <p><strong>Exact site of Tumour:</strong> {parsedData[section].DETAILS.PRIMARY_AND_EXACT_SITE_OF_TUMOUR}</p>
                )}
              </>
            )}

            {parsedData[section]?.CANCER_SURGERY_DETAILS && (
              <>
                <h5>Cancer Surgery Details</h5>
                <p><strong>Date of Surgery:</strong> {parsedData[section].CANCER_SURGERY_DETAILS?.DATE_OF_SURGERY || 'N/A'}</p>
                <p><strong>Nature of Surgery Performed:</strong> {parsedData[section].CANCER_SURGERY_DETAILS?.NATURE_OF_SURGERY_PERFORMED || 'N/A'}</p>
                <p><strong>Reason for performing surgery:</strong> {parsedData[section].CANCER_SURGERY_DETAILS?.REASON_FOR_PERFORMING_SURGERY || 'N/A'}</p>
              </>
            )}

            {parsedData[section]?.HEART_SURGERY_DETAILS && (
              <>
                <h5>Heart Surgery Details</h5>
                <p><strong>Details of Surgery Performed:</strong> {parsedData[section].HEART_SURGERY_DETAILS?.DETAILS_OF_SURGERY_OR_OTHER_TREATMENT_PERFORMED || 'N/A'}</p>
                <p><strong>Did patient suffer from Cardiomyopathy?:</strong> {parsedData[section].HEART_SURGERY_DETAILS?.PATIENT_SUFFERED_FROM_CARDIOMIOPATHY || 'N/A'}</p>
              </>
            )}
              </CollapsibleSection>

        )}

        {section === 'MEDICAL_TREATMENT_DETAILS' && (
        
          <>
            <h4>Medical Treatment Details</h4>
            <p><strong>Treatment Type:</strong> {parsedData[section]?.TREATMENT_TYPE || 'N/A'}</p>
            <p><strong>Treatment Duration:</strong> {parsedData[section]?.TREATMENT_DURATION || 'N/A'}</p>
          </>
        )}

        {section === 'HOSPITAL_DETAILS' && (
          <>
            <h4>Hospital Details</h4>
            <p><strong>Hospital Name:</strong> {parsedData[section]?.HOSPITAL_NAME || 'N/A'}</p>
            <p><strong>Location:</strong> {parsedData[section]?.LOCATION || 'N/A'}</p>
            <p><strong>Treating Physician:</strong> {parsedData[section]?.TREATING_PHYSICIAN || 'N/A'}</p>
          </>
        )}


            {/* Continue with other sections similarly */}
          </React.Fragment>
        ))}
      </div>
    );

  };

  return (
    <div className={styles.extractContentSection}>
      <div className={styles.extractHeader}>
        <h3 className={styles.extractTitle}>Extracted Content</h3>
      </div>

      {loading ? (
        <div className={styles.loadingState}>
          Loading extracted information...
        </div>
      ) : (
        renderReadableContent(data?.total_extracted_data)
      )}

      <div className={styles.reloadButtonContainer}>
        {rows.map((row, index) => (
          <button
            key={index}
            className={styles.reloadButton}
            onClick={() => handleReload(row.recNum)}
            disabled={loading}
          >
            <FontAwesomeIcon
              icon={faSync}
              spin={loading}
              className={styles.icon}
            />
            {!loading && "Reload Document"}
          </button>
        ))}
      </div>
    </div>
  );
};

export default ExtractedContent;


.extractContentSection {
  background: #f9fafb;
  padding: 20px;
  border: 1px solid #e1e4e8;
  border-radius: 12px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
  transition: all 0.3s ease;
}

.extractContentSection:hover {
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.08);
  transform: translateY(-2px);
}

.extractHeader {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 15px;
border-bottom: 2px solid transparent;
    border-image: linear-gradient(to right, #4a90e2, #50c878) 1;  padding-bottom: 10px;
}

.extractTitle {
  color: #2c3e50;
  font-size: 1.2rem;
  font-weight: 600;
  margin: 0;
  display: flex;
  align-items: center;
}

.extractTitle::before {
  content: '📋';
  margin-right: 10px;
  font-size: 1.5rem;
}



.readableContent {
  background: white;
  border-radius: 10px;
  padding: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
  line-height: 1.6;
  font-size: 14px;
  overflow-y: auto;
  max-height: 575px;
}

.loadingState {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 30px;
  color: #0f5fdc;
  font-weight: 500;
}

.noDataState {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 30px;
  background-color: #f0f4f8;
  border-radius: 10px;
  text-align: center;
}

.noDataIcon {
  font-size: 3rem;
  color: #0f5fdc;
  margin-bottom: 15px;
  opacity: 0.6;
}

.noDataText {
  color: #4a5568;
  font-size: 1rem;
  max-width: 300px;
}

.reloadButtonContainer {
  display: flex;
  justify-content: center;
  margin-top: 15px;
}

.reloadButton {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  padding: 10px 20px;
  background:linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: 500;
}

.reloadButton:hover {
  background-color: #1e40af;
  transform: translateY(-2px);
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.reloadButton:disabled {
  background-color: #a0aec0;
  cursor: not-allowed;
  transform: none;
}

.icon {
  font-size: 1.2rem;
}

.readableContent::-webkit-scrollbar {
  width: 8px;  /* Width of the scrollbar */
}

.readableContent::-webkit-scrollbar-track {
  background: #f1f1f1;  /* Background of the track */
  border-radius: 10px;
}

.readableContent::-webkit-scrollbar-thumb {
  background: #0f5fdc;  /* Color of the scrollbar thumb */
  border-radius: 10px;
  transition: background 0.3s ease;
}

.readableContent::-webkit-scrollbar-thumb:hover {
  background: #1e40af;  /* Darker shade on hover */
}


/* Enhanced Readable Content Styling */
.readableContent h4 {
  color: #2c3e50;
  /*border-bottom: 2px solid #0f5fdc;*/
  padding-bottom: 10px;
  margin-bottom: 15px;
  font-size: 1.1rem;
}

.readableContent h5 {
  color: #0f5fdc;
  margin-top: 15px;
  margin-bottom: 10px;
  font-size: 1rem;
}

.readableContent p {
  text-transform: capitalize;
  margin: 8px 0;
  color: #4a5568;
}

.readableContent strong {
  color: #2c3e50;
  font-weight: 600;
}

.sectionDivider {
  border-top: 1px solid #e2e8f0;
  margin: 15px 0;
  opacity: 0.5;
}

.collapsibleSection {
  background: #ffffff;
  border-radius: 8px;
  margin-bottom: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
  transition: all 0.3s ease;
}

.sectionHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 15px;
  cursor: pointer;
  background-color: #f9fafb;
  border-radius: 8px;
  transition: background-color 0.3s ease;
}

.sectionHeader:hover {
  background-color: #f0f4f8;
}

.sectionHeader h4 {
  margin: 0;
  color: #2c3e50;
  font-size: 1rem;
}

.toggleIcon {
  color: #0f5fdc;
  transition: transform 0.3s ease;
}

.sectionContent {
  padding: 15px;
  animation: fadeIn 0.3s ease;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}



import React, { useState, useEffect } from 'react';
import styles from './ClaimClassification.module.css';

const ClaimClassification = ({ data }) => {
  const [documentName, setDocumentName] = useState('');
  const [classification, setClassification] = useState('');
  const [isValid, setIsValid] = useState('');

  useEffect(() => {
    if (data) {
      try {
        // Data is already parsed from parent component - use directly
        setDocumentName(
          data.CLAIM_FORM_DETAILS?.DOCUMENT_NAME || 
          data.UPLOADED_DOCUMENT_NAME || 
          'Unnamed Document'
        );

        const formType = data.CLAIM_FORM_DETAILS?.CLAIM_FORM_TYPE || 'Unknown';
        setClassification(formType);

        // Corrected cardiomyopathy key spelling (added 'y' in CARDIOMYOPATHY)
        const cardiomyopathy = 
          data.DETAILS_OF_ILLNESS?.HEART_SURGERY_DETAILS?.PATIENT_SUFFERED_FROM_CARDIOMYOPATHY || 
          'N/A';
          
        const validationStatus = cardiomyopathy.toLowerCase() === 'no' ? 'Yes' : 'No';
        setIsValid(validationStatus);

      } catch (error) {
        console.error('Error processing classification data:', error);
        setDocumentName('Error Loading Document');
        setClassification('Error');
        setIsValid('Error');
      }
    } else {
      // Reset values if no data
      setDocumentName('');
      setClassification('');
      setIsValid('');
    }
  }, [data]);

  return (
    <div className={styles.claimClassificationContainer}>
      <h4>Claim Classification</h4>
      <table className={styles.classificationTable}>
        <thead>
          <tr>
            <th>Document Name</th>
            <th>Classification</th>
            <th>Is Valid?</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>{documentName}</td>
            <td>
              <span className={`${styles.classificationTag} ${
                classification.toLowerCase() === 'cancer' ? styles.cancerTag 
                : classification.toLowerCase() === 'heart' ? styles.heartTag 
                : styles.defaultTag
              }`}>
                {classification}
              </span>
            </td>
            <td>
              <span className={`${styles.validTag} ${
                isValid === 'Yes' ? styles.validYes : styles.validNo
              }`}>
                {isValid || 'N/A'}
              </span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  );
};

export default ClaimClassification;
.claimClassificationContainer {
  font-family: 'Inter', 'Arial', sans-serif;
  max-width: 700px;
  margin: 20px auto;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  border-radius: 12px;
  background-color: #ffffff;
  padding: 20px;
  transition: all 0.3s ease;
}

h4 {
  text-align: center;
  color: #2d3748;
  font-size: 18px;
  font-weight: 600;
  margin-bottom: 15px;
}

.classificationTable {
  width: 100%;
  border-collapse: collapse;
}

.classificationTable th, .classificationTable td {
  border: 1px solid #e1e8f0;
  padding: 12px;
  text-align: center;
}

.classificationTable th {
  background-color: #f0f4f8;
  font-weight: 600;
  color: #2d3748;
}

.classificationTable td {
  color: #4a5568;
}

/* Classification Tag Styles */
.classificationTag {
  display: inline-block;
  padding: 6px 12px;
  border-radius: 6px;
  font-size: 14px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.cancerTag {
  background-color: #ffebee;
  color: #d32f2f;
}

.heartTag {
  background-color: #e8f5e9;
  color: #2e7d32;
}

.defaultTag {
  background-color: #f5f5f5;
  color: #616161;
}

/* Validity Tag Styles */
.validTag {
  display: inline-block;
  padding: 6px 12px;
  border-radius: 6px;
  font-size: 14px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.validYes {
  background-color: #e8f5e9;
  color: #2e7d32;
}

.validNo {
  background-color: #ffebee;
  color: #d32f2f;
}

/* Responsive Design */
@media (max-width: 600px) {
  .claimClassificationContainer {
    max-width: 95%;
  }

  .classificationTable th, .classificationTable td {
    padding: 8px;
  }
}

