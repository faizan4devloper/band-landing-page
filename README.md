import React from "react";
import styles from "./ClaimProcessingStatus.module.css";

const ClaimProcessingStatus = ({ percentage, isLoading, dataExtracted }) => {
  const emptyKeyPercentage =
    percentage !== undefined ? Math.max(0, Math.min(100, parseFloat(percentage))) : 0;

  const filledKeyPercentage = 100 - emptyKeyPercentage;

  return (
    <div>
      <h3 className={styles.percentHead}>Claim Processing Status</h3>

      <div className={styles.percentageContainer}>
        <div className={styles.splitPercentageBar}>
          {!dataExtracted ? (
            /** Show animated loading effect while extraction is in progress */
            <div className={styles.loadingEffect}></div>
          ) : (
            /** Show the actual progress bar when extraction is done */
            <>
              <div
                className={styles.emptyKeysSection}
                style={{ width: `${emptyKeyPercentage}%` }}
              >
                {emptyKeyPercentage > 0 && `${emptyKeyPercentage.toFixed()}%`}
              </div>

              <div
                className={styles.filledKeysSection}
                style={{
                  width: emptyKeyPercentage === 0 ? "100%" : `${filledKeyPercentage}%`,
                }}
              >
                {filledKeyPercentage > 0 && `${filledKeyPercentage.toFixed()}%`}
              </div>
            </>
          )}
        </div>
      </div>
    </div>
  );
};

export default ClaimProcessingStatus;



/* Container */
.percentHead {
  color: #2c3e50;
  font-size: 1.2rem;
  font-weight: 600;
  margin: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  text-transform: uppercase;
}

/* Progress Bar Container */
.percentageContainer {
  width: 100%;
  height: 35px;
  background-color: #e0e0e0;
  border-radius: 20px;
  overflow: hidden;
  margin-top: 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  position: relative;
  display: flex;
  align-items: center;
}

/* Progress Bar */
.splitPercentageBar {
  display: flex;
  width: 100%;
  height: 100%;
  position: relative;
  border-radius: 20px;
}

/* Sections */
.emptyKeysSection,
.filledKeysSection {
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 600;
  font-size: 14px;
  transition: width 0.6s ease-in-out;
  white-space: nowrap;
  min-width: 0; /* Prevent text overflow */
  color: white;
}

.emptyKeysSection {
  background: linear-gradient(90deg, #ff6b6b, #ff3b3b);
}

.filledKeysSection {
  background: linear-gradient(90deg, #4ecb71, #2fa558);
}

/* Percentage Text */
.emptyKeysSection span,
.filledKeysSection span {
  padding: 5px 10px;
  border-radius: 15px;
  background: rgba(0, 0, 0, 0.2);
}

/* Loading Spinner */
.loadingSpinner {
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 14px;
  font-weight: bold;
  color: #2c3e50;
  margin-top: 12px;
}

.loadingSpinner::after {
  content: "";
  width: 15px;
  height: 15px;
  margin-left: 8px;
  border: 2px solid #0f5fdc;
  border-top: 2px solid transparent;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

/* Spinner Animation */
@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.loadingEffect {
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, #e0e0e0 25%, #f0f0f0 50%, #e0e0e0 75%);
  background-size: 200% 100%;
  animation: loadingAnimation 1.5s infinite linear;
}

@keyframes loadingAnimation {
  0% {
    background-position: 200% 0;
  }
  100% {
    background-position: -200% 0;
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

