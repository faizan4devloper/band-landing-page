i want table structure :- Document Name | Classification | Is Valid?

import React, { useState, useEffect } from 'react'; import styles from './ClaimClassification.module.css';

const ClaimClassification = ({ data }) => { const [documentName, setDocumentName] = useState(''); const [classification, setClassification] = useState(''); const [isValid, setIsValid] = useState(''); const [cardiomyopathyStatus, setCardiomyopathyStatus] = useState('');

useEffect(() => { if (data && data.total_extracted_data) { try { const parsedData = JSON.parse(data.total_extracted_data);

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

return ( <div className={styles.claimClassificationContainer}> <div className={styles.classificationTable}> <div className={styles.tableHeader}> <h4>Claim Classification</h4> </div> <div className={styles.tableBody}> <div className={styles.tableRow}> <div className={styles.tableLabel}>Document Name:</div> <div className={styles.tableValue}>{documentName}</div> </div> <div className={styles.tableRow}> <div className={styles.tableLabel}>Classification:</div> <div className={styles.tableValue}> <span className={\${styles.classificationTag} \${                   classification.toLowerCase() === 'cancer'                      ? styles.cancerTag                      : classification.toLowerCase() === 'heart'                     ? styles.heartTag                     : styles.defaultTag                 }} > {classification} </span> </div> </div> <div className={styles.tableRow}> <div className={styles.tableLabel}>Is Valid:</div> <div className={styles.tableValue}> <span className={\${styles.validTag} \${                   cardiomyopathyStatus.toLowerCase() === 'no'                      ? styles.validNo                      : cardiomyopathyStatus.toLowerCase() === 'yes'                     ? styles.validYes                     : styles.cardiomyopathyNA                 }} > {cardiomyopathyStatus} </span> </div> </div>

    </div>
  </div>
</div>
); };

export default ClaimClassification;



.claimClassificationContainer {
  font-family: 'Inter', 'Arial', sans-serif;
  max-width: 500px;
  margin: 20px auto;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  border-radius: 12px;
  background-color: #ffffff;
  transition: all 0.3s ease;
}

.classificationTable {
  border-radius: 12px;
  overflow: hidden;
}

.tableHeader {
  background-color: #f0f4f8;
  padding: 15px 20px;
  border-bottom: 1px solid #e1e8f0;
}

.tableHeader h4 {
  margin: 0;
  color: #2d3748;
  font-size: 18px;
  font-weight: 600;
}

.tableBody {
  padding: 20px;
}

.tableRow {
  display: flex;
  align-items: center;
  padding: 12px 0;
  border-bottom: 1px solid #edf2f7;
}

.tableRow:last-child {
  border-bottom: none;
}

.tableLabel {
  flex: 0 0 40%;
  font-weight: 500;
  color: #4a5568;
  padding-right: 15px;
}

.tableValue {
  flex: 1;
  color: #2d3748;
  font-weight: 400;
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

.surgeryTag {
  background-color: #e3f2fd;
  color: #1976d2;
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

.validNA {
  background-color: #f5f5f5;
  color: #616161;
}

/* Additional Details Section */
.additionalDetailsSection {
  background-color: #f9fafb;
  border-top: 1px solid #e1e8f0;
  padding: 15px 20px;
  border-radius: 0 0 12px 12px;
}

.additionalDetailsSection .tableRow {
  padding: 8px 0;
  border-bottom: 1px solid #e1e8f0;
}

.additionalDetailsSection .tableRow:last-child {
  border-bottom: none;
}

.additionalDetailsSection .tableLabel {
  color: #718096;
  font-size: 14px;
}

.additionalDetailsSection .tableValue {
  color: #2d3748;
  font-size: 14px;
}

/* Hover and Focus Effects */
.claimClassificationContainer:hover {
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.15);
  transform: translateY(-2px);
}

/* Responsive Design */
@media (max-width: 600px) {
  .claimClassificationContainer {
    max-width: 95%;
    margin: 10px auto;
  }

  .tableRow {
    flex-direction: column;
    align-items: flex-start;
  }

  .tableLabel {
    margin-bottom: 5px;
    width: 100%;
  }
}

/* Animation */
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.claimClassificationContainer {
  animation: fadeIn 0.5s ease-out;
}
