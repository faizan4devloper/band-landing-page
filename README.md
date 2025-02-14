
import React, { useState, useEffect } from 'react';
import styles from './ClaimClassification.module.css';

const ClaimClassification = ({ data }) => {
  const [documentName, setDocumentName] = useState('');
  const [classification, setClassification] = useState('');
  const [isValid, setIsValid] = useState('');

  useEffect(() => {
    if (data) {
      try {
        // Data is already parsed from parent component - use directly
        setDocumentName(
          data.CLAIM_FORM_DETAILS?.DOCUMENT_NAME || 
          data.UPLOADED_DOCUMENT_NAME || 
          'Unnamed Document'
        );

        const formType = data.CLAIM_FORM_DETAILS?.CLAIM_FORM_TYPE || 'Unknown';
        setClassification(formType);

        // Corrected cardiomyopathy key spelling (added 'y' in CARDIOMYOPATHY)
        const cardiomyopathy = 
          data.DETAILS_OF_ILLNESS?.HEART_SURGERY_DETAILS?.PATIENT_SUFFERED_FROM_CARDIOMYOPATHY || 
          'N/A';
          
        const validationStatus = cardiomyopathy.toLowerCase() === 'no' ? 'Yes' : 'No';
        setIsValid(validationStatus);

      } catch (error) {
        console.error('Error processing classification data:', error);
        setDocumentName('Error Loading Document');
        setClassification('Error');
        setIsValid('Error');
      }
    } else {
      // Reset values if no data
      setDocumentName('');
      setClassification('');
      setIsValid('');
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

    if (!claimFormPath) return;

    try {
      const payload = { claimid: currentClaimId, recnumber: currentClaimId, claimformpath: claimFormPath };
      const response = await axios.post("https:docembedqueue", payload, { 
        headers: { "Content-Type": "application/json" } 
      });
      setEmbeddingStatus(response.data);
    } catch (error) {
      console.error("Failed to call document embed queue:", error);
      setEmbeddingStatus(null);
    }
  };

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
        "https://percentage",
          { claimid: currentClaimId },
          { headers: { "Content-Type": "application/json" } }
        );
        if (response.data?.body) {
          let percentageValue = response.data.body.empty_key_perc 
            ? parseFloat(response.data.body.empty_key_perc.replace('%', ''))
            : 0;
          setPercentage(percentageValue);
        }
      } catch (error) {
        setPercentage(0);
      } finally {
        setIsLoading(false);
      }
    };
    fetchPercentage();
  }, [rows, data]);

  const handleVerify = (clid, formtype, summary, selectedPolicy) => {
    const policyMap = {
      'CANCER': 'PS391481',
      'HEART': 'PS672908',
      'default': 'PS672908'
    };
    navigate('/verify', {
      state: { 
        clid, 
        summary: summary || "No Summary Available", 
        selectedPolicy: policyMap[formtype] || policyMap.default, 
        percentage 
      },
    });
  };

  const handleReload = async (recNum) => {
    setLoading(true);
    try {
      const response = await axios.post(
          "https://all", 
        { tasktype: "FETCH_SINGLE_ACT_CLAIM", claimid: recNum }, 
        { headers: { "Content-Type": "application/json" } }
      );
      setData(response.data.allclaimactdata);
    } catch (error) {
      console.error("Failed to fetch data:", error);
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
          <div className={`${styles.cardContainer} ${styles.claimClassificationContainer}`}>
            <ClaimClassification
              data={data?.total_extracted_data ? JSON.parse(data.total_extracted_data) : null}
              uploadedFileName={uploadedFileName}
            />
          </div>
          
          <div className={`${styles.cardContainer} ${styles.documentPreviewContainer}`}>
            <DocumentPreview staticPreviewUrl={staticPreviewUrl} />
          </div>
        </div>

        <div className={styles.rightColumn}>
          <div className={`${styles.cardContainer} ${styles.percentageSection}`}>
            <ClaimProcessingStatus 
              percentage={percentage} 
              isLoading={isLoading} 
              dataExtracted={!!data}
            />
          </div>

          <div className={`${styles.cardContainer} ${styles.extractedContentContainer}`}>
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
              Verify Claim <FontAwesomeIcon icon={faChevronRight} />
            </button>
          </div>
        </div>
      </div>
    </div>
  );
};

export default MainContent;
