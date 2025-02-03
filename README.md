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
