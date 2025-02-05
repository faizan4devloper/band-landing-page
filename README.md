
import React, { useState, useEffect } from 'react';
import styles from './ClaimClassification.module.css';

const ClaimClassification = ({ data, uploadedFileName }) => {
  const [documentName, setDocumentName] = useState('Unknown Document');
  const [classification, setClassification] = useState('Unknown');
  const [isValid, setIsValid] = useState('No');

  useEffect(() => {
    console.log("Received Data:", data);

    const processData = () => {
      try {
        if (uploadedFileName) {
          setDocumentName(uploadedFileName);
          return;
        }

        if (!data || !data.total_extracted_data) {
          console.warn("No extracted data found");
          return;
        }

        let parsedData;
        try {
          parsedData = JSON.parse(data.total_extracted_data);
        } catch (parseError) {
          console.error('Error parsing JSON:', parseError);
          return;
        }

        console.log("Parsed Data:", parsedData);

        // Extract Document Name
        const extractedDocumentName = 
          parsedData.CLAIM_FORM_DETAILS?.DOCUMENT_NAME ||
          parsedData.UPLOADED_DOCUMENT_NAME ||
          documentName;
        setDocumentName(extractedDocumentName);

        // Extract Classification from CLAIM_FORM_TYPE
        const extractedClassification = 
          parsedData.CLAIM_FORM_DETAILS?.CLAIM_FORM_TYPE || 
          parsedData.CLASSIFICATION ||
          classification;
        setClassification(extractedClassification);

        // Check if DETAILS_OF_ILLNESS exists
        const isValidClaim = parsedData.DETAILS_OF_ILLNESS ? 'Yes' : 'No';
        setIsValid(isValidClaim);

      } catch (error) {
        console.error('Error processing data:', error);
        setDocumentName('Unknown Document');
        setClassification('Unknown');
        setIsValid('No');
      }
    };

    processData();
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
              <span className={`${styles.classificationTag} ${
                classification.toLowerCase() === 'cancer' ? styles.cancerTag :
                classification.toLowerCase() === 'heart' ? styles.heartTag :
                styles.defaultTag
              }`}>
                {classification}
              </span>
            </td>
            <td>
              <span className={`${styles.validTag} ${
                isValid === 'Yes' ? styles.validYes : styles.validNo
              }`}>
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
