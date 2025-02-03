
i want table style and structure like that | doc name | classification | is Valid | :-

import React, { useState, useEffect } from 'react';
import styles from './ClaimClassification.module.css';

const ClaimClassification = ({ data }) => {
  const [documentName, setDocumentName] = useState('');
  const [classification, setClassification] = useState('');
  const [isValid, setIsValid] = useState('');
  const [cardiomyopathyStatus, setCardiomyopathyStatus] = useState('');

  useEffect(() => {
    if (data && data.total_extracted_data) {
      try {
        const parsedData = JSON.parse(data.total_extracted_data);
        
        // Extract document name
        setDocumentName(
          parsedData.CLAIM_FORM_DETAILS?.DOCUMENT_NAME || 
          parsedData.UPLOADED_DOCUMENT_NAME || 
          'Unnamed Document'
        );

        // Determine classification
        const formType = parsedData.CLAIM_FORM_DETAILS?.CLAIM_FORM_TYPE || 'Unknown';
        setClassification(formType);

        // Determine Cardiomyopathy status
        const cardiomyopathy = parsedData.DETAILS_OF_ILLNESS?.HEART_SURGERY_DETAILS?.PATIENT_SUFFERED_FROM_CARDIOMIOPATHY || 'N/A';
        setCardiomyopathyStatus(cardiomyopathy);

        // Determine validity based on Cardiomyopathy status
        const validationStatus = cardiomyopathy.toLowerCase() === 'no' ? 'Yes' : 'No';
        setIsValid(validationStatus);

      } catch (error) {
        console.error('Error parsing extracted data:', error);
        setDocumentName('Unknown Document');
        setClassification('Unknown');
        setIsValid('Error');
        setCardiomyopathyStatus('N/A');
      }
    }
  }, [data]);

  return (
    <div className={styles.claimClassificationContainer}>
      <div className={styles.classificationTable}>
        <div className={styles.tableHeader}>
          <h4>Claim Classification</h4>
        </div>
        <div className={styles.tableBody}>
          <div className={styles.tableRow}>
            <div className={styles.tableLabel}>Document Name:</div>
            <div className={styles.tableValue}>{documentName}</div>
          </div>
          <div className={styles.tableRow}>
            <div className={styles.tableLabel}>Classification:</div>
            <div className={styles.tableValue}>
              <span 
                className={`${styles.classificationTag} ${
                  classification.toLowerCase() === 'cancer' 
                    ? styles.cancerTag 
                    : classification.toLowerCase() === 'heart'
                    ? styles.heartTag
                    : styles.defaultTag
                }`}
              >
                {classification}
              </span>
            </div>
          </div>
          <div className={styles.tableRow}>
            <div className={styles.tableLabel}>Is Valid:</div>
            <div className={styles.tableValue}>
              <span 
                className={`${styles.validTag} ${
                  cardiomyopathyStatus.toLowerCase() === 'no' 
                    ? styles.validNo 
                    : cardiomyopathyStatus.toLowerCase() === 'yes'
                    ? styles.validYes
                    : styles.cardiomyopathyNA
                }`}
              >
                {cardiomyopathyStatus}
              </span>
            </div>
          </div>
          
        </div>
      </div>
    </div>
  );
};

export default ClaimClassification;


.claimClassificationContainer {
  width: 100%;
  max-width: 500px;
  margin: 0 auto;
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.08);
  overflow: hidden;
  transition: all 0.3s ease;
}

.claimClassificationContainer:hover {
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.12);
  transform: translateY(-5px);
}

.classificationTable {
  width: 100%;
}

.tableHeader {
  background: linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
  color: white;
  padding: 15px 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.tableHeader h4 {
  margin: 0;
  font-size: 1.1rem;
  font-weight: 600;
  letter-spacing: 0.5px;
}

.tableBody {
  padding: 20px;
}

.tableRow {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 0;
  border-bottom: 1px solid #f0f0f0;
  transition: background-color 0.2s ease;
}

.tableRow:last-child {
  border-bottom: none;
}

.tableRow:hover {
  background-color: #f9f9f9;
}

.tableLabel {
  font-weight: 600;
  color: #4a5568;
  font-size: 0.9rem;
  flex: 1;
}

.tableValue {
  color: #2d3748;
  font-size: 0.95rem;
  text-align: right;
  flex: 1.5;
}

.classificationTag,
.validTag {
  padding: 6px 12px;
  border-radius: 20px;
  font-size: 0.8rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  display: inline-block;
  min-width: 100px;
  text-align: center;
}

/* Classification Tags */
.cancerTag {
  background-color: #ffebee;
  color: #d32f2f;
}

.heartTag {
  background-color: #e3f2fd;
  color: #1976d2;
}

.defaultTag {
  background-color: #f0f4f8;
  color: #4a5568;
}

/* Validation Tags */
.validYes {
  background-color: #e8f5e9;
  color: #2e7d32;
}

.validNo {
  background-color: #ffebee;
  color: #d32f2f;
}

.validPending {
  background-color: #fff3e0;
  color: #ef6c00;
}

/* Responsive Adjustments */
@media screen and (max-width: 600px) {
  .claimClassificationContainer {
    max-width: 100%;
    margin: 0 10px;
  }

  .tableRow {
    flex-direction: column;
    align-items: flex-start;
    padding: 15px 0;
  }

  .tableLabel,
  .tableValue {
    width: 100%;
    text-align: left;
    margin-bottom: 5px;
  }

  .tableValue {
    text-align: left;
  }

  .classificationTag,
  .validTag {
    width: 100%;
    text-align: center;
  }
}

/* Accessibility and Print Styles */
@media print {
  .claimClassificationContainer {
    box-shadow: none;
    border: 1px solid #e0e0e0;
  }
}
