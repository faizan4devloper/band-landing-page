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
        "https:/percentage",
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
          "https:/all", 
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


.mainContentGrid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); /* More responsive */
  gap: 1rem;
  padding: 1rem;
  height: auto; /* Remove fixed height */
}

.cardContainer {
  background: white;
  border-radius: 8px;
  padding: 1.25rem;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  margin-bottom: 1rem;
}


.claimIdDisplay {
  margin: 1rem 0;
  padding: 0.5rem 1rem;
  background: #f8f9fa;
  border-radius: 6px;
}


.leftColumn, .rightColumn {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-unit);
}

/* Card Layout Template */
.cardContainer {
  /*background: var(--text-light);*/
  /*border-radius: var(--border-radius);*/
  /*border: 1px solid var(--border-color);*/
  /*box-shadow: var(--box-shadow);*/
  /*padding: var(--spacing-unit);*/
  transition: transform 0.2s ease;
}

.cardContainer:hover {
  transform: translateY(-2px);
}

/* Specific Component Containers */
.claimClassificationContainer {
  max-height: 400px;
  overflow: auto;
}

.compact-table {
  font-size: 0.875rem;
  border-collapse: collapse;
}
.compact-table td, .compact-table th {
  padding: 0.5rem;
  border-bottom: 1px solid #eee;
}
.documentPreviewContainer {
  max-height: 500px;
  min-height: 300px;
  position: relative;
}

.percentageSection {
  height: auto; /* Remove fixed height */
  padding: 1rem;
}


.extractedContentContainer {
  flex: 1;
  min-height: 400px;
}

/* Verify Button Styling */
.verifyButton {
  padding: 0.75rem 1.5rem;
  font-size: 1rem;
  border-radius: 6px;
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

