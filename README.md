import React from "react";
import styles from "./ClaimProcessingStatus.module.css";

const ClaimProcessingStatus = ({ percentage, isLoading, dataExtracted }) => {
  const progress = Math.max(0, Math.min(100, parseFloat(percentage))) || 0;

  return (
    <div className={styles.container}>
      <div className={styles.header}>
        <h3>Processing Status</h3>
        {dataExtracted && <span className={styles.percentage}>{progress}%</span>}
      </div>
      
      <div className={styles.progressContainer}>
        {!dataExtracted ? (
          <div className={styles.loadingBar} />
        ) : (
          <div 
            className={styles.progressBar}
            style={{ width: `${progress}%` }}
          >
            <div className={styles.progressFill} />
          </div>
        )}
      </div>
      
      <div className={styles.statusLabels}>
        <span>Initial Scan</span>
        <span>Validation Complete</span>
      </div>
    </div>
  );
};

export default ClaimProcessingStatus;


/* ClaimProcessingStatus.module.css */
.container {
  background: #ffffff;
  border-radius: 16px;
  padding: 1.5rem;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
  transition: transform 0.2s ease;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
}

h3 {
  color: #2d3748;
  font-size: 1.1rem;
  font-weight: 600;
  margin: 0;
}

.percentage {
  background: #4a5568;
  color: white;
  padding: 0.25rem 0.75rem;
  border-radius: 20px;
  font-size: 0.9rem;
}

.progressContainer {
  height: 12px;
  background: #edf2f7;
  border-radius: 20px;
  overflow: hidden;
  position: relative;
}

.progressBar {
  height: 100%;
  position: relative;
  transition: width 0.8s ease-out;
}

.progressFill {
  height: 100%;
  background: linear-gradient(90deg, #48bb78 0%, #38a169 100%);
  border-radius: 20px;
  box-shadow: 0 2px 8px rgba(72, 187, 120, 0.2);
  animation: pulse 2s infinite;
}

.loadingBar {
  height: 100%;
  width: 100%;
  background: linear-gradient(90deg, #edf2f7 25%, #e2e8f0 50%, #edf2f7 75%);
  background-size: 200% 100%;
  animation: loading 1.5s infinite linear;
}

.statusLabels {
  display: flex;
  justify-content: space-between;
  margin-top: 0.75rem;
  color: #718096;
  font-size: 0.85rem;
}

@keyframes loading {
  0% { background-position: 200% 0; }
  100% { background-position: -200% 0; }
}

@keyframes pulse {
  0% { opacity: 0.9; }
  50% { opacity: 0.7; }
  100% { opacity: 0.9; }
}



import React, { useEffect, useState } from 'react';
import styles from './ClaimClassification.module.css';

const ClaimClassification = ({ data }) => {
  const [documentInfo, setDocumentInfo] = useState({
    name: '',
    type: '',
    isValid: false
  });

  useEffect(() => {
    if (data) {
      const classification = data.CLAIM_FORM_DETAILS?.CLAIM_FORM_TYPE || 'Unknown';
      const isValid = classification === 'HEART' 
        ? (data.DETAILS_OF_ILLNESS?.HEART_SURGERY_DETAILS?.PATIENT_SUFFERED_FROM_CARDIOMYOPATHY || '').toLowerCase() === 'no'
        : true;

      setDocumentInfo({
        name: data.CLAIM_FORM_DETAILS?.DOCUMENT_NAME || 'Unnamed Document',
        type: classification,
        isValid
      });
    }
  }, [data]);

  return (
    <div className={styles.container}>
      <div className={styles.header}>
        <h3>Document Classification</h3>
        <span className={styles.badge} data-type={documentInfo.type.toLowerCase()}>
          {documentInfo.type || 'N/A'}
        </span>
      </div>
      
      <div className={styles.content}>
        <div className={styles.infoRow}>
          <label>Document Name:</label>
          <p>{documentInfo.name}</p>
        </div>
        
        <div className={styles.statusIndicator}>
          <div className={styles.indicatorDot} data-valid={documentInfo.isValid} />
          <span>{documentInfo.isValid ? 'Valid Structure' : 'Validation Required'}</span>
        </div>
      </div>
    </div>
  );
};

export default ClaimClassification;
