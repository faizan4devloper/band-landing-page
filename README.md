import React from "react";
import styles from "./ClaimProcessingStatus.module.css";
import { motion } from "framer-motion";

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
          <motion.div
            className={styles.loadingBar}
            initial={{ opacity: 0.5 }}
            animate={{ opacity: 1 }}
            transition={{ repeat: Infinity, duration: 1.5 }}
          />
        ) : (
          <motion.div
            className={styles.progressBar}
            initial={{ width: 0 }}
            animate={{ width: `${progress}%` }}
            transition={{ duration: 0.8 }}
          >
            <div className={styles.progressFill} />
          </motion.div>
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



/* ClaimProcessingStatus.module.css */
.container {
  background: #ffffff;
  border-radius: 16px;
  padding: 1.5rem;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
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
}

.progressFill {
  height: 100%;
  background: linear-gradient(90deg, #48bb78 0%, #38a169 100%);
  border-radius: 20px;
  box-shadow: 0 2px 8px rgba(72, 187, 120, 0.2);
}

.loadingBar {
  height: 100%;
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





/* ClaimClassification.module.css */
.container {
  background: #ffffff;
  border-radius: 16px;
  padding: 1.5rem;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
  transition: transform 0.2s ease;
}

.container:hover {
  transform: translateY(-2px);
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1.25rem;
}

h3 {
  color: #2d3748;
  font-size: 1.1rem;
  font-weight: 600;
  margin: 0;
}

.badge {
  padding: 0.35rem 1rem;
  border-radius: 20px;
  font-size: 0.85rem;
  font-weight: 600;
  text-transform: uppercase;
}

.badge[data-type="cancer"] {
  background: #fff5f5;
  color: #c53030;
}

.badge[data-type="heart"] {
  background: #f0fff4;
  color: #2f855a;
}

.badge[data-type="unknown"] {
  background: #edf2f7;
  color: #4a5568;
}

.content {
  display: grid;
  gap: 1rem;
}

.infoRow {
  display: grid;
  grid-template-columns: 120px 1fr;
  align-items: baseline;
}

label {
  color: #718096;
  font-size: 0.9rem;
}

p {
  margin: 0;
  color: #2d3748;
  font-weight: 500;
  word-break: break-word;
}

.statusIndicator {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  margin-top: 1rem;
}

.indicatorDot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: #48bb78;
}

.indicatorDot[data-valid="false"] {
  background: #f56565;
}

@media (max-width: 768px) {
  .infoRow {
    grid-template-columns: 1fr;
    gap: 0.25rem;
  }
  
  .badge {
    font-size: 0.75rem;
  }
}
