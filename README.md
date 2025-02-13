

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


import React from 'react';
import styles from "./DocumentPreview.module.css";
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { 
  faCloudUploadAlt, 
  faFilePdf, 
  faFileAlt, 
  faEye 
} from '@fortawesome/free-solid-svg-icons';

const DocumentPreview = ({ staticPreviewUrl, onUpload }) => {
  const handleUploadClick = () => {
    if (onUpload) {
      onUpload();
    }
  };

  return (
    <div className={styles.previewSection}>
      <div className={styles.previewHeader}>
        <h3 className={styles.previewTitle}>
          Document Preview
        </h3>
        
        {staticPreviewUrl && (
          <div className={styles.previewActions}>
            <FontAwesomeIcon 
              icon={faEye} 
              className={styles.previewIcon} 
              title="View Document"
            />
          </div>
        )}
      </div>

      {staticPreviewUrl ? (
        <iframe
        
          src={staticPreviewUrl}
          title="Static Document Preview"
          className={styles.documentPreviewIframe}
        ></iframe>
      ) : (
        <div className={styles.noDocument}>
          <FontAwesomeIcon 
            icon={faFilePdf} 
            className={styles.noDocumentIcon} 
          />
          <p className={styles.noDocumentText}>
            No document has been uploaded yet. Please upload a PDF to preview.
          </p>
          {onUpload && (
            <button 
              className={styles.uploadButton}
              onClick={handleUploadClick}
            >
              <FontAwesomeIcon 
                icon={faCloudUploadAlt} 
                className={styles.uploadIcon} 
              />
              Upload Document
            </button>
          )}
        </div>
      )}
    </div>
  );
};

export default DocumentPreview;

.previewSection {
  background: #f9fafb;
  padding: 20px;
  border: 1px solid #e1e4e8;
  border-radius: 12px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
  transition: all 0.3s ease;
}

.previewSection:hover {
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.08);
  transform: translateY(-2px);
}

.previewHeader {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 15px;
border-bottom: 2px solid transparent;
    border-image: linear-gradient(to right, #4a90e2, #50c878) 1;
    padding-bottom: 10px;
}

.previewTitle {
  color: #2c3e50;
  font-size: 1.2rem;
  font-weight: 600;
  margin: 0;
  display: flex;
  align-items: center;
}

.previewTitle::before {
  content: '📄';
  margin-right: 10px;
  font-size: 1.5rem;
}

.documentPreviewIframe {
  width: 100%;
  height: 600px;
  border: none;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  transition: box-shadow 0.3s ease;
}

.documentPreviewIframe:hover {
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
}

.noDocument {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 40px;
  background-color: #f0f4f8;
  border-radius: 12px;
  text-align: center;
}

.noDocumentIcon {
  font-size: 4rem;
  color: #0f5fdc;
  margin-bottom: 20px;
  opacity: 0.7;
}

.noDocumentText {
  color: #4a5568;
  font-size: 1rem;
  max-width: 300px;
  line-height: 1.6;
}

.uploadButton {
  margin-top: 20px;
  padding: 10px 20px;
  background-color: #0f5fdc;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  transition: all 0.3s ease;
  font-weight: 500;
}

.uploadButton:hover {
  background-color: #1e40af;
  transform: translateY(-2px);
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}


