import React, { useState, useEffect } from 'react';
import styles from './ClaimClassification.module.css';

const ClaimClassification = ({ data }) => {
  const [documentName, setDocumentName] = useState('');
  const [classification, setClassification] = useState('');
  const [isValid, setIsValid] = useState('');

  useEffect(() => {
    if (data && data.total_extracted_data) {
      try {
        const parsedData = JSON.parse(data.total_extracted_data);
        
        setDocumentName(
          parsedData.CLAIM_FORM_DETAILS?.DOCUMENT_NAME || 
          parsedData.UPLOADED_DOCUMENT_NAME || 
          'Unnamed Document'
        );

        setClassification(parsedData.CLAIM_FORM_DETAILS?.CLAIM_FORM_TYPE || 'Unknown');

        const cardiomyopathy = parsedData.DETAILS_OF_ILLNESS?.HEART_SURGERY_DETAILS?.PATIENT_SUFFERED_FROM_CARDIOMIOPATHY || 'N/A';
        setIsValid(cardiomyopathy.toLowerCase() === 'no' ? 'Yes' : 'No');
      } catch (error) {
        console.error('Error parsing extracted data:', error);
        setDocumentName('Unknown Document');
        setClassification('Unknown');
        setIsValid('Error');
      }
    }
  }, [data]);

  return (
    <div className={styles.claimClassificationContainer}>
      <table className={styles.classificationTable}>
        <thead>
          <tr>
            <th>Doc Name</th>
            <th>Classification</th>
            <th>Is Valid</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>{documentName}</td>
            <td className={
              classification.toLowerCase() === 'cancer' ? styles.cancerTag :
              classification.toLowerCase() === 'heart' ? styles.heartTag :
              styles.defaultTag
            }>
              {classification}
            </td>
            <td className={isValid === 'Yes' ? styles.validYes : styles.validNo}>
              {isValid}
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  );
};

export default ClaimClassification;

/* ClaimClassification.module.css */

.claimClassificationContainer {
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  overflow: hidden;
}

.classificationTable {
  width: 100%;
  border-collapse: collapse;
}

.classificationTable th, .classificationTable td {
  padding: 12px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

.classificationTable th {
  background-color: #007bff;
  color: white;
}

.classificationTable tr:hover {
  background-color: #f1f1f1;
}

.cancerTag {
  background-color: #ffebee;
  color: #d32f2f;
  padding: 5px 10px;
  border-radius: 5px;
}

.heartTag {
  background-color: #e3f2fd;
  color: #1976d2;
  padding: 5px 10px;
  border-radius: 5px;
}

.defaultTag {
  background-color: #f0f4f8;
  color: #4a5568;
  padding: 5px 10px;
  border-radius: 5px;
}

.validYes {
  background-color: #e8f5e9;
  color: #2e7d32;
  padding: 5px 10px;
  border-radius: 5px;
}

.validNo {
  background-color: #ffebee;
  color: #d32f2f;
  padding: 5px 10px;
  border-radius: 5px;
}
