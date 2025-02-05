
i want display data from extractedContent to ClaimClassification in classification column will display the type from extraction and in the is Valid column display the if the Details of Illness is present then yes and then not no:- import React, { useState, useEffect } from 'react'; import styles from './ClaimClassification.module.css';

const ClaimClassification = ({ data, uploadedFileName }) => { const [documentName, setDocumentName] = useState('Unknown Document'); const [classification, setClassification] = useState('Unknown'); const [isValid, setIsValid] = useState('No');

useEffect(() => { console.log("Received Data:", data);

const processData = () => {
  try {
    // Handle uploaded file name first
    if (uploadedFileName) {
      setDocumentName(uploadedFileName);
      return;
    }

    // Check if data exists and has total_extracted_data
    if (!data || !data.total_extracted_data) {
      console.warn("No extracted data found");
      return;
    }

    // Safely parse the JSON data
    let parsedData;
    try {
      parsedData = JSON.parse(data.total_extracted_data);
    } catch (parseError) {
      console.error('Error parsing JSON:', parseError);
      return;
    }

    console.log("Parsed Data:", parsedData);

    // Extract Document Name with multiple fallback options
    const extractedDocumentName = 
      parsedData.CLAIM_FORM_DETAILS?.DOCUMENT_NAME ||
      parsedData.UPLOADED_DOCUMENT_NAME ||
      documentName;
    setDocumentName(extractedDocumentName);

    // Extract Classification
    const extractedClassification = 
      parsedData.CLAIM_FORM_DETAILS?.CLAIM_FORM_TYPE || 
      parsedData.CLASSIFICATION ||
      classification;
    setClassification(extractedClassification);

    // Validate Claim
    const validateClaim = () => {
      // Multiple validation checks
      const validationChecks = [
        // Check Cardiomyopathy
        parsedData.DETAILS_OF_ILLNESS?.HEART_SURGERY_DETAILS?.PATIENT_SUFFERED_FROM_CARDIOMIOPATHY,
        // Add more validation checks as needed
        parsedData.IS_VALID,
        parsedData.VALIDATION_STATUS
      ];

      // Return 'Yes' if any validation passes, otherwise 'No'
      const validationResult = validationChecks.some(check => 
        check && 
        (check.toLowerCase() === 'no' || 
         check.toLowerCase() === 'valid' || 
         check === true)
      ) ? 'Yes' : 'No';

      return validationResult;
    };

    const validationStatus = validateClaim();
    setIsValid(validationStatus);

  } catch (error) {
    console.error('Comprehensive error processing data:', error);
    // Reset to default values on error
    setDocumentName('Unknown Document');
    setClassification('Unknown');
    setIsValid('No');
  }
};

// Call the processing function
processData();
}, [data, uploadedFileName]);

return ( <div className={styles.claimClassificationContainer}> <h4>Claim Classification</h4> <table className={styles.classificationTable}> <thead> <tr> <th>Document Name</th> <th>Classification</th> <th>Is Valid?</th> </tr> </thead> <tbody> <tr> <td>{documentName}</td> <td> <span className={\${styles.classificationTag} \${                   classification.toLowerCase() === 'cancer' ? styles.cancerTag :                   classification.toLowerCase() === 'heart' ? styles.heartTag :                   styles.defaultTag                 }} > {classification} </span> </td> <td> <span className={\${styles.validTag} \${                   isValid === 'Yes' ? styles.validYes : styles.validNo                 }} > {isValid} </span> </td> </tr> </tbody> </table> </div> ); };

export default ClaimClassification;

import React from 'react'; import { FontAwesomeIcon } from '@fortawesome/react-fontawesome'; import { faSync, faFileAlt } from '@fortawesome/free-solid-svg-icons'; import styles from "./ExtractedContent.module.css";

const ExtractedContent = ({ data, loading, rows, handleReload }) => { const renderReadableContent = (data) => { if (!data) return ( <div className={styles.noDataState}> <FontAwesomeIcon icon={faFileAlt} className={styles.noDataIcon} /> <p className={styles.noDataText}> Processing the document. Please wait... </p> </div> );

const parsedData = JSON.parse(data);
console.log('Extraction ParsedData', parsedData)
return (

<div className={styles.readableContent}> {Object.keys(parsedData).map((section, index) => ( <React.Fragment key={index}> {index > 0 && <div className={styles.sectionDivider}></div>}
    {section === 'CLAIM_FORM_DETAILS' && (
      <>
        <h4>Claim Form Details</h4>
        <p><strong>Type:</strong> {parsedData[section]?.CLAIM_FORM_TYPE || 'Not specified'}</p>
      </>
    )}

    {section === 'CLINICAL_ABSTRACT_APPLICATION' && (
      <>
        <h4>Clinical Abstract Application</h4>
        <p><strong>Name of Patient:</strong> {parsedData[section]?.NAME_OF_PATIENT || 'N/A'}</p>
        <p><strong>NRIC/FIN/BC:</strong> {parsedData[section]?.NRIC_FIN_BC || 'N/A'}</p>
        <p><strong>Address:</strong> {parsedData[section]?.ADDRESS || 'N/A'}</p>
        <p><strong>Date:</strong> {parsedData[section]?.DATE || 'N/A'}</p>
      </>
    )}

    {section === 'CLAIMANT_STATEMENT' && (
      <>
        <h4>Claimant Statement</h4>
        {parsedData[section]?.POLICY_DETAILS && (
          <>
            <h5>Policy Details</h5>
            <p><strong>Policy Numbers:</strong> {parsedData[section].POLICY_DETAILS?.POLICY_NUMBERS || 'N/A'}</p>
            <p><strong>Type of Claim:</strong> {parsedData[section].POLICY_DETAILS?.TYPE_OF_CLAIM || 'N/A'}</p>
          </>
        )}

        {parsedData[section]?.DETAILS_OF_LIFE_ASSURED && (
          <>
            <h5>Details of Life Assured</h5>
            <p><strong>Name:</strong> {parsedData[section].DETAILS_OF_LIFE_ASSURED?.NAME_OF_LIFE_ASSURED || 'N/A'}</p>
            <p><strong>Gender:</strong> {parsedData[section].DETAILS_OF_LIFE_ASSURED?.GENDER || 'N/A'}</p>
            <p><strong>Marital Status:</strong> {parsedData[section].DETAILS_OF_LIFE_ASSURED?.MARITAL_STATUS || 'N/A'}</p>
          </>
        )}
      </>
    )}

    {section === 'DETAILS_OF_ILLNESS' && (
      <>
        <h4>Details of Illness</h4>
        {parsedData[section]?.DETAILS && (
          <>
            <p><strong>Date when patient first consulted:</strong> {parsedData[section].DETAILS?.DATE_PATIENT_FIRST_CONSULTED || 'N/A'}</p>
            <p><strong>Symptoms presented at first consultation:</strong> {parsedData[section].DETAILS?.DETAILS_OF_SYMPTOMS_PRESENTED_AT_FIRST_CONSULTATION || 'N/A'}</p>
            <p><strong>Underlying of Symptoms:</strong> {parsedData[section].DETAILS?.UNDERLYING_OF_SYMPTOMS || 'N/A'}</p>
            <p><strong>Exact Diagnosis of Condition:</strong> {parsedData[section].DETAILS?.EXACT_DIAGNOSIS_OF_CONDITION || 'N/A'}</p>
            
            {parsedData[section].DETAILS?.PRIMARY_AND_EXACT_SITE_OF_TUMOUR && (
              <p><strong>Exact site of Tumour:</strong> {parsedData[section].DETAILS.PRIMARY_AND_EXACT_SITE_OF_TUMOUR}</p>
            )}
          </>
        )}

        {parsedData[section]?.CANCER_SURGERY_DETAILS && (
          <>
            <h5>Cancer Surgery Details</h5>
            <p><strong>Date of Surgery:</strong> {parsedData[section].CANCER_SURGERY_DETAILS?.DATE_OF_SURGERY || 'N/A'}</p>
            <p><strong>Nature of Surgery Performed:</strong> {parsedData[section].CANCER_SURGERY_DETAILS?.NATURE_OF_SURGERY_PERFORMED || 'N/A'}</p>
            <p><strong>Reason for performing surgery:</strong> {parsedData[section].CANCER_SURGERY_DETAILS?.REASON_FOR_PERFORMING_SURGERY || 'N/A'}</p>
          </>
        )}

        {parsedData[section]?.HEART_SURGERY_DETAILS && (
          <>
            <h5>Heart Surgery Details</h5>
            <p><strong>Details of Surgery Performed:</strong> {parsedData[section].HEART_SURGERY_DETAILS?.DETAILS_OF_SURGERY_OR_OTHER_TREATMENT_PERFORMED || 'N/A'}</p>
            <p><strong>Did patient suffer from Cardiomyopathy?:</strong> {parsedData[section].HEART_SURGERY_DETAILS?.PATIENT_SUFFERED_FROM_CARDIOMIOPATHY || 'N/A'}</p>
          </>
        )}
      </>
    )}

    {section === 'MEDICAL_TREATMENT_DETAILS' && (
      <>
        <h4>Medical Treatment Details</h4>
        <p><strong>Treatment Type:</strong> {parsedData[section]?.TREATMENT_TYPE || 'N/A'}</p>
        <p><strong>Treatment Duration:</strong> {parsedData[section]?.TREATMENT_DURATION || 'N/A'}</p>
      </>
    )}

    {section === 'HOSPITAL_DETAILS' && (
      <>
        <h4>Hospital Details</h4>
        <p><strong>Hospital Name:</strong> {parsedData[section]?.HOSPITAL_NAME || 'N/A'}</p>
        <p><strong>Location:</strong> {parsedData[section]?.LOCATION || 'N/A'}</p>
        <p><strong>Treating Physician:</strong> {parsedData[section]?.TREATING_PHYSICIAN || 'N/A'}</p>
      </>
    )}
  </React.Fragment>
))}
</div> );
};

return ( <div className={styles.extractContentSection}> <div className={styles.extractHeader}> <h3 className={styles.extractTitle}>Extracted Content</h3>

  </div>

  {loading ? (
    <div className={styles.loadingState}>
      Loading extracted information...
    </div>
  ) : (
    renderReadableContent(data?.total_extracted_data)
  )}

  <div className={styles.reloadButtonContainer}>
    {rows.map((row, index) => (
      <button
        key={index}
        className={styles.reloadButton}
        onClick={() => handleReload(row.recNum)}
        disabled={loading}
      >
        <FontAwesomeIcon
          icon={faSync}
          spin={loading}
          className={styles.icon}
        />
        {!loading && "Reload Document"}
      </button>
    ))}
  </div>
</div>
); };

export default ExtractedContent;

import React, { useState, useEffect } from "react"; import axios from "axios"; import Modal from "react-modal"; import styles from "./MainContent.module.css"; import { useNavigate } from "react-router-dom"; import { FontAwesomeIcon } from '@fortawesome/react-fontawesome'; import { faSync, faChevronRight } from '@fortawesome/free-solid-svg-icons';

import DocumentPreview from "./DocumentPreview"; import ExtractedContent from "./ExtractedContent"; import ClaimClassification from "./ClaimClassification"; import ClaimProcessingStatus from "./ClaimProcessingStatus";

const MainContent = ({ message, rows, clid, setRows, staticPreviewUrl, selectedPolicy, uploadedFileName }) => { const navigate = useNavigate(); const [loading, setLoading] = useState(false);

const [data, setData] = useState(null);
const [percentage, setPercentage] = useState(null);
const [isLoading, setIsLoading] = useState(false);
  const [embeddingStatus, setEmbeddingStatus] = useState(null);


// Existing useEffects (fetchPercentage and data loading)
useEffect(() => {
  if (rows.length > 0) {
    handleReload(rows[0].recNum);
  }
}, [rows]);

const handleDocumentEmbedQueue = async () => {
if (rows.length === 0) return;

const currentClaimId = rows[0].recNum;
const claimFormPath = data?.total_extracted_data 
  ? JSON.parse(data?.total_extracted_data).CLAIM_FORM_DETAILS?.CLAIM_FORM_PATH 
  : null;

if (!claimFormPath) {
  console.warn('No claim form path found');
  return;
}

try {
  const payload = {
    claimid: currentClaimId,
    recnumber: currentClaimId,
    claimformpath: claimFormPath
  };

  const response = await axios.post(
    "dummy",
    payload,
  
    { 
      headers: { 
        "Content-Type": "application/json" 
      } 
    }
  );

  console.log('Document Embed Queue Response:', response);
  
  // Optional: Set embedding status
  setEmbeddingStatus(response.data);
} catch (error) {
  console.error("Failed to call document embed queue:", error);
  setEmbeddingStatus(null);
}
};

// Trigger document embedding when data is loaded useEffect(() => { if (data && data.total_extracted_data) { handleDocumentEmbedQueue(); } }, [data]);

useEffect(() => { const fetchPercentage = async () => { if (rows.length === 0 || !rows[0]?.recNum) return;

const currentClaimId = rows[0].recNum;
setIsLoading(true);
try {
  const response = await axios.post(
    "dummy",
    { claimid: currentClaimId },
    { headers: { "Content-Type": "application/json" } }
  );

  if (response.data && response.data.body) {
    let percentageValue = response.data.body.empty_key_perc 
      ? parseFloat(response.data.body.empty_key_perc.replace('%', ''))
      : 0;
      
    setPercentage(percentageValue);
  }
} catch (error) {
  setPercentage(0);  // Set to 0 if error occurs
} finally {
  setIsLoading(false);
}
};

fetchPercentage(); }, [rows, data]);

const handleVerify = (clid, formtype, summary, selectedPolicy) => {
  if (formtype === 'CANCER') {
    selectedPolicy = 'PS391481';
  } else if (formtype === 'HEART') {
    selectedPolicy = 'PS672908';
  } else {
    selectedPolicy = 'PS672908';
  }

  navigate('/verify', {
    state: { clid, summary, selectedPolicy },
  });
};

const handleReload = async (recNum) => {
  setLoading(true);
  try {
    const payload = {
      tasktype: "FETCH_SINGLE_ACT_CLAIM",
      claimid: recNum,
    };

    const response = await axios.post(
      "dummy", 
      payload, 
      { headers: { "Content-Type": "application/json" } }
    );
    
    console.log('Extraction:', response)
    
    setData(response.data.allclaimactdata);
  } catch (error) {
    console.error("Failed to fetch data for RecNum:", recNum, error);
  } finally {
    setLoading(false);
  }
};

return (
  <div className={styles.mainContentWrapper}>
    {rows.length > 0 && (
      <div className={styles.claimIdDisplay}>
        <h3>Claim ID: {rows[0].recNum}</h3>
      </div>
    )}
    
    <div className={styles.mainContentGrid}>
      <div className={styles.leftColumn}>
        <div className={styles.claimClassificationContainer}>
          <ClaimClassification data={data} uploadedFileName={uploadedFileName}/>
        </div>
        <div className={styles.documentPreviewContainer}>
          <DocumentPreview staticPreviewUrl={staticPreviewUrl} />
        </div>
        
        
      </div>
      
      

      <div className={styles.rightColumn}>
        <div className={styles.percentageSection}>
{isLoading ? ( <p>Loading percentage...</p> // Show loading text or a spinner ) : ( <ClaimProcessingStatus percentage={percentage} isLoading={isLoading} /> )}

</div>
        <div className={styles.extractedContentContainer}>
          <ExtractedContent 
            data={data} 
            loading={loading} 
            rows={rows} 
            handleReload={handleReload}
          />
        </div>
        
        <div className={styles.verifyButtonContainer}>
          <button
            className={styles.verifyButton}
            onClick={() =>
              handleVerify(
                clid,
                data?.total_extracted_data
                  ? JSON.parse(data?.total_extracted_data).CLAIM_FORM_DETAILS?.CLAIM_FORM_TYPE
                  : "No Claim Form Type",
                data?.total_extracted_data
                  ? JSON.parse(data?.total_extracted_data).CLAIM_FORM_DETAILS?.CLAIM_FORM_DETAILED_SUMMARY
                  : "No Summary Available",
                selectedPolicy
              )
            }
            disabled={!rows.length}
          >
            Verify Claim <FontAwesomeIcon icon={faChevronRight}/>
          </button>
        </div>
      </div>
    </div>
  </div>
);
};

export default MainContent;
