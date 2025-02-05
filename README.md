import React, { useState, useEffect } from 'react';
import styles from './ClaimClassification.module.css';

const ClaimClassification = ({ data, uploadedFileName }) => {
  const [documentName, setDocumentName] = useState('');
  const [classification, setClassification] = useState('');
  const [isValid, setIsValid] = useState('');

  useEffect(() => {
    if (uploadedFileName) {
      setDocumentName(uploadedFileName);
    } else if (data && data.total_extracted_data) {
      try {
        const parsedData = JSON.parse(data.total_extracted_data);
        setDocumentName(
          parsedData.CLAIM_FORM_DETAILS?.DOCUMENT_NAME || 
          parsedData.UPLOADED_DOCUMENT_NAME || 
          'Unnamed Document'
        );

        // Determine classification
        const formType = parsedData.CLAIM_FORM_DETAILS?.CLAIM_FORM_TYPE || 'Unknown';
        setClassification(formType);

        // Determine validity based on Cardiomyopathy status
        const cardiomyopathy = parsedData.DETAILS_OF_ILLNESS?.HEART_SURGERY_DETAILS?.PATIENT_SUFFERED_FROM_CARDIOMIOPATHY || 'N/A';
        const validationStatus = cardiomyopathy.toLowerCase() === 'no' ? 'Yes' : 'No';
        setIsValid(validationStatus);

      } catch (error) {
        console.error('Error parsing extracted data:', error);
        setDocumentName('Unknown Document');
        setClassification('Unknown');
        setIsValid('Error');
      }
    }
  }, [data, uploadedFileName]);

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
              <span className={`${styles.classificationTag} ${classification.toLowerCase() === 'cancer' ? styles.cancerTag 
                : classification.toLowerCase() === 'heart' ? styles.heartTag 
                : styles.defaultTag}`}>
                {classification}
              </span>
            </td>
            <td>
              <span className={`${styles.validTag} ${isValid === 'Yes' ? styles.validYes : styles.validNo}`}>
                {isValid}
              </span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  );
};

export default ClaimClassification;




