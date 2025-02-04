i want add beutiful consistent styling on the table:- import React, { useState, useEffect } from 'react'; import styles from './ClaimClassification.module.css';

const ClaimClassification = ({ data }) => { const [documentName, setDocumentName] = useState(''); const [classification, setClassification] = useState(''); const [isValid, setIsValid] = useState('');

useEffect(() => { if (data && data.total_extracted_data) { try { const parsedData = JSON.parse(data.total_extracted_data);

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

return ( <div className={styles.claimClassificationContainer}> <table className={styles.classificationTable}> <thead> <tr> <th>Doc Name</th> <th>Classification</th> <th>Is Valid</th> </tr> </thead> <tbody> <tr> <td>{documentName}</td> <td className={ classification.toLowerCase() === 'cancer' ? styles.cancerTag : classification.toLowerCase() === 'heart' ? styles.heartTag : styles.defaultTag }> {classification} </td> <td className={isValid === 'Yes' ? styles.validYes : styles.validNo}> {isValid} </td> </tr> </tbody> </table> </div> ); };

export default ClaimClassification;

.claimClassificationContainer { width: 100%; max-width: 800px; margin: 20px auto; box-shadow: 0 4px 16px rgba(0, 0, 0, 0.1); border-radius: 12px; overflow: hidden; background-color: #ffffff; transition: all 0.3s ease; }

.classificationTable { width: 100%; border-collapse: separate; border-spacing: 0; font-family: 'Inter', 'Arial', sans-serif; }

/* Table Header Styles */ .classificationTable thead { background-color: #f0f4f8; }

.classificationTable thead tr { height: 50px; }

.classificationTable thead th { text-align: left; padding: 12px 15px; color: #2d3748; font-weight: 600; text-transform: uppercase; letter-spacing: 0.5px; border-bottom: 2px solid #e2e8f0; }

/* Table Body Styles */ .classificationTable tbody tr { transition: background-color 0.2s ease; }

.classificationTable tbody tr:hover { background-color: #f7fafc; }

.classificationTable tbody td { padding: 15px; border-bottom: 1px solid #e2e8f0; color: #4a5568; font-weight: 500; }

.classificationTable tbody tr:last-child td { border-bottom: none; }

/* Classification Tag Styles */ .classificationTag { display: inline-block; padding: 6px 12px; border-radius: 6px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.5px; font-size: 0.8rem; }

.cancerTag { background-color: #ffebee; color: #d32f2f; }

.heartTag { background-color: #e8f5e9; color: #2e7d32; }

.defaultTag { background-color: #f0f0f0; color: #616161; }

/* Validity Tag Styles */ .validYes { background-color: #e8f5e9; color: #2e7d32; font-weight: 600; padding: 6px 12px; border-radius: 6px; text-transform: uppercase; letter-spacing: 0.5px; font-size: 0.8rem; }

.validNo { background-color: #ffebee; color: #d32f2f; font-weight: 600; padding: 6px 12px; border-radius: 6px; text-transform: uppercase; letter-spacing: 0.5px; font-size: 0.8rem; }

/* Responsive Design */ @media (max-width: 600px) { .claimClassificationContainer { margin: 10px; width: calc(100% - 20px); }

.classificationTable thead th, .classificationTable tbody td { padding: 10px; } }

/* Animations */ @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

.claimClassificationContainer { animation: fadeIn 0.5s ease-out; }

/* Enhanced Hover Effect */ .classificationTable tbody tr { position: relative; overflow: hidden; }

.classificationTable tbody tr::after { content: ''; position: absolute; left: 0; bottom: 0; width: 0; height: 2px; background-color: #3182ce; transition: width 0.3s ease; }

.classificationTable tbody tr:hover::after { width: 100%; }
