
import React, { useState, useEffect } from 'react';
import styles from './ClaimClassification.module.css';

const ClaimClassification = ({ data, uploadedFileName }) => {
  const [documentName, setDocumentName] = useState('');
  const [classification, setClassification] = useState('');
  const [isValid, setIsValid] = useState('');


useEffect(() => {
  if (data) {
    try {
      setDocumentName(
        data.CLAIM_FORM_DETAILS?.DOCUMENT_NAME || 
        data.UPLOADED_DOCUMENT_NAME || 
        uploadedFileName || 
        'Unnamed Document'
      );

      setClassification(data.CLAIM_FORM_DETAILS?.CLAIM_FORM_TYPE || 'Unknown');

      // Check if DETAILS_OF_ILLNESS exists and has content
      const hasIllnessDetails = data.DETAILS_OF_ILLNESS && Object.keys(data.DETAILS_OF_ILLNESS).length > 0;
      setIsValid(hasIllnessDetails ? 'Yes' : 'No');

    } catch (error) {
      console.error('Error processing classification data:', error);
      setDocumentName('Error Loading Document');
      setClassification('Error');
      setIsValid('Error');
    }
  } else {
    setDocumentName('');
    setClassification('');
    setIsValid('');
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
  /*font-family: 'Inter', 'Arial', sans-serif;*/
  max-width: 700px;
  /*margin: 20px auto;*/
  margin:0;
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

