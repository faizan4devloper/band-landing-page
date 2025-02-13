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
          "https:///percentage",
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



/* MainContent.module.css */
:root {
  --primary-color: #2c3e50;
  --secondary-color: #3498db;
  --background-light: #f8f9fa;
  --border-color: #ecf0f1;
  --text-dark: #2c3e50;
  --text-light: #ffffff;
  --spacing-unit: 1rem;
  --border-radius: 12px;
  --box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  --transition-timing: cubic-bezier(0.4, 0, 0.2, 1);
}

.mainContentWrapper {
  padding: 1.5rem;
  height: 100vh;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  background-color: var(--background-light);
}

.claimIdDisplay {
  position: sticky;
  top: 0;
  z-index: 100;
  background: rgba(255, 255, 255, 0.8);
  backdrop-filter: blur(10px);
  padding: 1rem;
  border-radius: var(--border-radius);
  box-shadow: var(--box-shadow);
  margin-bottom: 1rem;
}

.mainContentGrid {
  display: flex;
  gap: 1.5rem;
  height: calc(100vh - 160px);
}

.leftColumn {
  flex: 0 0 380px;
  display: grid;
  grid-template-rows: auto 1fr;
  gap: 1.5rem;
}

.rightColumn {
  flex: 1;
  display: grid;
  grid-template-rows: auto 1fr auto;
  gap: 1.5rem;
}

.cardContainer {
  background: white;
  border-radius: var(--border-radius);
  padding: 1.5rem;
  box-shadow: var(--box-shadow);
  border: 1px solid var(--border-color);
  transition: transform 0.2s var(--transition-timing);
}

.cardContainer:hover {
  transform: translateY(-2px);
}

.documentPreviewContainer {
  overflow-y: auto;
  min-height: 300px;
}

.extractedContentContainer {
  overflow-y: auto;
  max-height: 60vh;
}

.percentageSection {
  padding: 1rem;
  background: linear-gradient(135deg, #f8f9fa 0%, #ffffff 100%);
}

.verifyButtonContainer {
  margin-top: auto;
  padding-top: 1rem;
}

.verifyButton {
  background-color: var(--secondary-color);
  color: var(--text-light);
  border: none;
  padding: 0.75rem 2rem;
  border-radius: var(--border-radius);
  cursor: pointer;
  width: 100%;
  font-size: 1.1rem;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.75rem;
  transition: all 0.3s var(--transition-timing);
  position: relative;
  overflow: hidden;
}

.verifyButton:after {
  content: "";
  position: absolute;
  left: -100%;
  top: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(120deg, transparent, rgba(255, 255, 255, 0.4), transparent);
  transition: all 0.8s;
}

.verifyButton:hover {
  background-color: #2980b9;
  transform: scale(1.02);
}

.verifyButton:hover:after {
  left: 100%;
}

@media (max-width: 1200px) {
  .mainContentGrid {
    flex-direction: column;
    height: auto;
  }
  
  .leftColumn {
    grid-template-columns: 1fr 1fr;
    grid-template-rows: auto;
    flex: none;
    gap: 1rem;
  }
  
  .documentPreviewContainer {
    min-height: 400px;
  }
}

@media (max-width: 768px) {
  .leftColumn {
    grid-template-columns: 1fr;
  }
  
  .mainContentWrapper {
    padding: 1rem;
  }
  
  .cardContainer {
    padding: 1rem;
  }
}

@keyframes spin {
  to { transform: rotate(360deg); }
}
