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

        // Extract document name
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
  }, [data]);

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



.claimClassificationContainer {
  font-family: 'Inter', 'Arial', sans-serif;
  max-width: 700px;
  margin: 20px auto;
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









import React from 'react';
import styles from "./ClaimProcessingStatus.module.css";

const ClaimProcessingStatus = ({ percentage, isLoading }) => {
  // Ensure percentage is a number, default to 0 if not
  const processedPercentage = percentage !== undefined 
    ? Math.max(0, Math.min(100, parseFloat(percentage)))
    : 0;

  return (
    <div className={styles.container}>
      <h3 className={styles.percentHead}>Claim Processing Status</h3>

      {isLoading ? (
        <div className={styles.loadingContainer}>
          <div className={styles.spinner}></div>
          <p className={styles.loadingText}>Processing claim status...</p>
        </div>
      ) : (
        <div className={styles.percentageContainer}>
          <div className={styles.progressBar}>
            <div
              className={styles.filledSection}
              style={{ width: `${processedPercentage}%` }}
            >
              {processedPercentage > 0 && `${processedPercentage.toFixed()}%`}
            </div>
          </div>
        </div>
      )}
    </div>
  );
};

export default ClaimProcessingStatus;








why the percentage only display 100% on the verifyContent screen i mean statically not dynamically engaging with ClaimProcessingStatus

import React from 'react';
import styles from "./ClaimProcessingStatus.module.css";

const ClaimProcessingStatus = ({ percentage, isLoading }) => {
  // Ensure percentage is a number, default to 0 if not
  const emptyKeyPercentage = percentage !== undefined 
    ? Math.max(0, Math.min(100, parseFloat(percentage)))
    : 0;
  
  const filledKeyPercentage = 100 - emptyKeyPercentage;

  return (
    <div>
      <h3 className={styles.percentHead}>Claim Processing Status</h3>

      {isLoading ? (
        <div className={styles.loadingSpinner}>Processing claim status...</div>
      ) : (
        <div className={styles.percentageContainer}>
          <div className={styles.splitPercentageBar}>
            <div
              className={styles.emptyKeysSection}
              style={{ width: `${emptyKeyPercentage}%` }}
            >
              {emptyKeyPercentage > 0 && `${emptyKeyPercentage.toFixed()}%`}
            </div>

            <div
              className={styles.filledKeysSection}
              style={{
                width: emptyKeyPercentage === 0 ? "100%" : `${filledKeyPercentage}%`,
              }}
            >
              {filledKeyPercentage > 0 && `${filledKeyPercentage.toFixed()}%`}
            </div>
          </div>
        </div>
      )}
    </div>
  );
};

export default ClaimProcessingStatus;



  
  import React, { useState, useEffect } from "react";
  import axios from "axios";
  import Modal from "react-modal";
  import styles from "./MainContent.module.css";
  import { useNavigate } from "react-router-dom";
  import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
  import { faSync, faChevronRight } from '@fortawesome/free-solid-svg-icons';
  
  
  import DocumentPreview from "./DocumentPreview";
  import ExtractedContent from "./ExtractedContent";
  import ClaimClassification from "./ClaimClassification";
  import ClaimProcessingStatus from "./ClaimProcessingStatus";
  
  
  const MainContent = ({ message, rows, clid, setRows, staticPreviewUrl, selectedPolicy, uploadedFileName }) => {
    const navigate = useNavigate();
    const [loading, setLoading] = useState(false);

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
        "https://docembedqueue",
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

  // Trigger document embedding when data is loaded
  useEffect(() => {
    if (data && data.total_extracted_data) {
      handleDocumentEmbedQueue();
    }
  }, [data]);

  
useEffect(() => {
  const fetchPercentage = async () => {
    if (rows.length === 0 || !rows[0]?.recNum) return;

    const currentClaimId = rows[0].recNum;
    setIsLoading(true);
    try {
      const response = await axios.post(
        "https:/percentage",
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

  fetchPercentage();
}, [rows, data]);

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
          "https:all", 
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
<ClaimClassification 
  data={data?.total_extracted_data ? JSON.parse(data.total_extracted_data) : null} 
  uploadedFileName={uploadedFileName} 
/>
            </div>
            <div className={styles.documentPreviewContainer}>
              <DocumentPreview staticPreviewUrl={staticPreviewUrl} />
            </div>
            
            
          </div>
          
          
  
          <div className={styles.rightColumn}>
            <div className={styles.percentageSection}>
  {isLoading ? (
    <p>Loading percentage...</p>  // Show loading text or a spinner
  ) : (
    <ClaimProcessingStatus 
      percentage={percentage} 
      isLoading={isLoading} 
    />
  )}
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


import React, { useEffect, useState } from "react";
import { useLocation, useNavigate } from "react-router-dom";
import VerifyContent from "./VerifyContent";
import useVerifyData from "./useVerifyData";
import BreadCrumbs from "../../BreadCrumbs/BreadCrumbs";

const Verify = () => {
  const navigate = useNavigate();
  const location = useLocation();

  const breadcrumbItems = [
    { 
      label: 'Claims', 
      link: '/manage-claims' 
    },
    { 
      label: 'New Claim', 
      link: '/new-claim' 
    },
    { label: 'Verify' }
  ];

  // Retrieve claim data from location state or localStorage
  const [recNum, setRecNum] = useState(() => 
    location.state?.clid || 
    localStorage.getItem('currentClaimId')
  );

  const [psid, setPsid] = useState(() => 
    location.state?.selectedPolicy || 
    localStorage.getItem('currentPolicyId')
  );

  const [Dsummary, setDsummary] = useState(() => 
    location.state?.summary || 
    localStorage.getItem('currentSummary') || 
    "No Summary Available"
  );

  // Save to localStorage whenever values change
  useEffect(() => {
    if (recNum) localStorage.setItem('currentClaimId', recNum);
    if (psid) localStorage.setItem('currentPolicyId', psid);
    if (Dsummary) localStorage.setItem('currentSummary', Dsummary);
  }, [recNum, psid, Dsummary]);

  // Validate claim ID
  // useEffect(() => {
  //   if (!recNum) {
  //     navigate('/manage-claims', { 
  //       state: { 
  //         error: 'No claim ID provided. Please start a new claim.' 
  //       } 
  //     });
  //   }
  // }, [recNum, navigate]);

  // Fetch verify data
  const { summary, recommendation, loading, error } = useVerifyData(recNum);

  // Clear storage when component unmounts
  useEffect(() => {
    return () => {
      localStorage.removeItem('currentClaimId');
      localStorage.removeItem('currentPolicyId');
      localStorage.removeItem('currentSummary');
    };
  }, []);

  // Handle no claim ID scenario
  if (!recNum) {
    return null;
  }


  return (
    <div>
      <BreadCrumbs items={breadcrumbItems} />
      <VerifyContent 
        recNum={recNum}
        psid={psid}
        Dsummary={Dsummary}
        summary={summary}
        recommendation={recommendation}
        loading={loading}
        error={error}
      />
    </div>
  );
};

export default Verify;


import React from "react";
import { useNavigate } from "react-router-dom";
import { HashLoader } from "react-spinners";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faEnvelope, faAnglesRight } from "@fortawesome/free-solid-svg-icons";
import styles from "./Verify.module.css";
import Chatbot from "./Chatbot";
import ClaimProcessingStatus from "../MainContent/ClaimProcessingStatus";
import VerificationDB from "./VerificationDB";
import Insights from "./Insights"; // Import the Insights component

const VerifyContent = ({
  recNum,
  psid,
  Dsummary,
  summary,
  recommendation,
  loading,
  error,
  emptyKeysPercentage,
}) => {
  const navigate = useNavigate();

  const HandleEmail = () => {
    navigate("/generate-email", {
      state: {
        Dsummary,
        summary,
        recommendation,
        recNum,
        psid,
      },
    });
  };

  // Render loading state
  if (loading) {
    return (
      <div className={styles.spinnerContainer}>
        <HashLoader color="#0f5fdc" size={40} />
      </div>
    );
  }

  // Render error state
  if (error) {
    return (
      <div className={styles.errorContainer}>
        <p>Error: {error}</p>
      </div>
    );
  }

  // Render no data state
  if (!summary || !recommendation) {
    return (
      <div className={styles.noDataContainer}>
        <p>No verification data available.</p>
      </div>
    );
  }

  return (
  <div className={styles.verifyMainContainer}>
    {/* Claim ID and Status Section */}
    <div className={styles.headerSection}>
      <div className={styles.claimIdDisplay}>
        <div className={styles.claimIdBadge}>
          <span>Claim ID</span>
          <h3>{recNum || 'N/A'}</h3>
        </div>
        <div className={styles.claimIdBadge}>
          <span>PS ID</span>
          <h3>{psid || 'N/A'}</h3>
        </div>
      </div>
      <div className={styles.statusSection}>
        <ClaimProcessingStatus percentage={emptyKeysPercentage} isLoading={loading} />
      </div>
    </div>

    {/* Main Content Layout */}
    <div className={styles.verifyContainer}>
      <div>
        <Chatbot />
      </div>

      {/* Left and Right Panels */}
      <div className={styles.rightLeft}>
        {/* Claim Summary */}
        <div className={styles.leftPanel}>
          <div className={styles.panelHeader}>
            <h3>Claim Summary</h3>
          </div>
          <div className={styles.panelContent}>
            <p>{Dsummary || 'No summary available'}</p>
          </div>
        </div>

        {/* Verification Summary */}
        <div className={styles.rightPanel}>
          <div className={styles.panelHeader}>
            <h3>Verification Summary</h3>
          </div>
          <div className={styles.panelContent}>
            <div className={styles.summarySection}>
              <h4>Detailed Recommendation</h4>
              <p>{recommendation || 'No detailed recommendation available'}</p>
            </div>
            <div className={styles.claimDetails}>
              <div className={styles.claimDetailItem}>
                <strong>Claim Type:</strong>
                <span>{summary?.claimType || 'Not specified'}</span>
              </div>
              <div className={styles.claimDetailItem}>
                <strong>Claim Status:</strong>
                <span>{summary?.claimStatus || 'Pending'}</span>
              </div>
            </div>
          </div>
        </div>
      </div>

      {/* Insights & VerificationDB (Inline Layout) */}
      <div className={styles.bottomPanel}>
        <div className={styles.insightsPanel}>
          <div className={styles.panelHeader}>
            <h3>Insights From Historic Claims
</h3>
          </div>
          <Insights claimType={summary?.claimType || 'GENERAL'} />
        </div>
        <div className={styles.verificationDBPanel}>
          <div className={styles.panelHeader}>
            <h3>Verification With Integrated System</h3>
          </div>
          <VerificationDB recNum={recNum} psid={psid} />
        </div>
      </div>
    </div>

    {/* Generate Email Button */}
    <div className={styles.generateEmailContainer}>
      <button 
        className={styles.generateEmailButton} 
        onClick={HandleEmail}
        disabled={!summary || !recommendation}
      >
        <FontAwesomeIcon icon={faEnvelope} className={styles.emailIcon} />
          Generate Email <FontAwesomeIcon icon={faAnglesRight} />
      </button>
    </div>
  </div>
);
};

export default VerifyContent;







/* MainContent.module.css */
/* Design System Variables */
:root {
  --primary-color: #2c3e50;
  --secondary-color: #3498db;
  --background-light: #f8f9fa;
  --border-color: #ecf0f1;
  --text-dark: #2c3e50;
  --text-light: #ffffff;
  --spacing-unit: 1rem;
  --border-radius: 8px;
  --box-shadow: 0 2px 15px rgba(0, 0, 0, 0.1);
}

.mainContentWrapper {
  padding: calc(var(--spacing-unit) * 2);
  height: 100vh;
  overflow-y: auto;
}

.claimIdDisplay {
  background-color: var(--primary-color);
  color: var(--text-light);
  padding: var(--spacing-unit);
  border-radius: var(--border-radius);
  margin-bottom: var(--spacing-unit);
  box-shadow: var(--box-shadow);
}

.mainContentGrid {
  display: grid;
  grid-template-columns: 1fr 1.5fr;
  gap: calc(var(--spacing-unit) * 2);
  height: calc(100% - 60px);
}

.leftColumn, .rightColumn {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-unit);
}

/* Card Layout Template */
.cardContainer {
  background: var(--text-light);
  border-radius: var(--border-radius);
  border: 1px solid var(--border-color);
  box-shadow: var(--box-shadow);
  padding: var(--spacing-unit);
  transition: transform 0.2s ease;
}

.cardContainer:hover {
  transform: translateY(-2px);
}

/* Specific Component Containers */
.claimClassificationContainer {
  flex: 0 1 auto;
}

.documentPreviewContainer {
  flex: 1 1 50%;
  min-height: 400px;
}

.percentageSection {
  height: 120px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.extractedContentContainer {
  flex: 1;
  min-height: 400px;
}

/* Verify Button Styling */
.verifyButtonContainer {
  margin-top: auto;
  padding-top: var(--spacing-unit);
}

.verifyButton {
  background-color: var(--secondary-color);
  color: var(--text-light);
  border: none;
  padding: calc(var(--spacing-unit) * 0.75) calc(var(--spacing-unit) * 2);
  border-radius: var(--border-radius);
  cursor: pointer;
  width: 100%;
  font-size: 1.1rem;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: var(--spacing-unit);
  transition: all 0.3s ease;
}

.verifyButton:hover {
  background-color: #2980b9;
  transform: scale(1.02);
}

.verifyButton:disabled {
  background-color: #95a5a6;
  cursor: not-allowed;
  opacity: 0.7;
}

/* Loading States */
.loadingText {
  color: var(--secondary-color);
  display: flex;
  align-items: center;
  gap: var(--spacing-unit);
}

/* Responsive Design */
@media (max-width: 1200px) {
  .mainContentGrid {
    grid-template-columns: 1fr;
    height: auto;
  }

  .leftColumn, .rightColumn {
    min-height: auto;
  }

  .documentPreviewContainer {
    min-height: 300px;
  }
}




// In your JSX
<div className={`${styles.claimClassificationContainer} ${styles.cardContainer}`}>
  <ClaimClassification ... />
</div>

<div className={`${styles.documentPreviewContainer} ${styles.cardContainer}`}>
  <DocumentPreview ... />
</div>

<div className={`${styles.percentageSection} ${styles.cardContainer}`}>
  {/* percentage content */}
</div>



{isLoading && (
  <div className={styles.loadingText}>
    <FontAwesomeIcon icon={faSync} spin />
    Loading...
  </div>
)}
