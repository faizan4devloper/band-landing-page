
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

const MainContent = ({ message, rows, clid, setRows, staticPreviewUrl, selectedPolicy }) => {
  const navigate = useNavigate();
  const [loading, setLoading] = useState(false);
  const [data, setData] = useState(null);
  const [percentage, setPercentage] = useState(null);
  const [isLoading, setIsLoading] = useState(false);

  // Existing useEffects (fetchPercentage and data loading)
  useEffect(() => {
    if (rows.length > 0) {
      handleReload(rows[0].recNum);
    }
  }, [rows]);

  useEffect(() => {
    const fetchPercentage = async () => {
      if (clid) {
        setIsLoading(true);
        try {
          const response = await axios.post(
            "dummy",
            { claimid: clid }
          );

          let percentageValue = response.data?.body?.empty_key_perc 
            ? parseFloat(response.data.body.empty_key_perc) 
            : 0;

          setPercentage(percentageValue);
        } catch (error) {
          console.error("Error fetching percentage:", error);
          setPercentage(0);
        } finally {
          setIsLoading(false);
        }
      }
    };

    fetchPercentage();
  }, [clid]);

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
          <div className={styles.documentPreviewContainer}>
            <DocumentPreview staticPreviewUrl={staticPreviewUrl} />
          </div>
          
          <div className={styles.claimClassificationContainer}>
            <ClaimClassification data={data} />
          </div>
        </div>
        
        <div className={styles.percentageSection}>
          <ClaimProcessingStatus 
            percentage={percentage} 
            isLoading={isLoading} 
          />
        </div>

        <div className={styles.rightColumn}>
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


/* Global Styles */
.mainContentWrapper {
  /*width: 100%;*/
  max-width: 1400px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f4f6f9;
}

/* Claim ID Display */
.claimIdDisplay {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  padding: 15px;
  background-color: #ffffff;
  border-radius: 12px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
}

.claimIdDisplay h3 {
  margin: 0;
  color: #2c3e50;
  font-size: 1.2rem;
  font-weight: 600;
}

/* Main Content Grid Layout */
.mainContentGrid {
  display: grid;
  grid-template-columns: 1.3fr 1fr;
  gap: 24px;
}

/* Left Column Styles */
.leftColumn {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.documentPreviewContainer,
.claimClassificationContainer {
  background-color: #ffffff;
  border-radius: 16px;
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.08);
  /*padding: 24px;*/
  transition: all 0.3s ease;
}

.documentPreviewContainer:hover,
.claimClassificationContainer:hover {
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
  transform: translateY(-5px);
}

/* Right Column Styles */
.rightColumn {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.extractedContentContainer {
  background-color: #ffffff;
  border-radius: 16px;
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.08);
  /*padding: 24px;*/
  /*max-height: 650px;*/
  /*overflow-y: auto;*/
  transition: all 0.3s ease;
}

.extractedContentContainer:hover {
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
  transform: translateY(-5px);
}

/* Verify Button Styles */
.verifyButtonContainer {
  display: flex;
  justify-content: center;
  margin-top: 16px;
}

.verifyButton {
     width: 100%;
    padding: 12px 20px;
    font-size: 14px;
    font-weight: 700;
    text-transform: uppercase;
    background: linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
    color: white;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    transition: all 0.4s ease;
    letter-spacing: 1px;
    box-shadow: 0 6px 12px rgba(74, 144, 226, 0.3);
}

.verifyButton:hover {
 
  transform: translateY(-3px);
  box-shadow: 0 8px 16px rgba(74, 144, 226, 0.4);
}

.verifyButton:active {
  transform: translateY(1px);
  box-shadow: 0 4px 8px rgba(74, 144, 226, 0.2);
}

.verifyButton:disabled {
  background: linear-gradient(135deg, #a0a0a0 0%, #c0c0c0 100%);
  cursor: not-allowed;
  box-shadow: none;
}

/* Modal Styles */
.modal {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #ffffff;
  border-radius: 16px;
  padding: 30px;
  max-width: 500px;
  width: 100%;
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.1);
}

.modalOverlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

/* Scrollbar Styles */

/* Responsive Design */
@media screen and (max-width: 1200px) {
  .mainContentGrid {
    grid-template-columns: 1fr;
  }
}

@media screen and (max-width: 768px) {
  .mainContentWrapper {
    padding: 10px;
  }

  .claimIdDisplay {
    flex-direction: column;
    text-align: center;
  }

  .documentPreviewContainer,
  .claimClassificationContainer,
  .extractedContentContainer {
    padding: 15px;
  }
}


