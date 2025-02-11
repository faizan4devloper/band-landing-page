import React, { useState } from "react";
import { useNavigate } from "react-router-dom";
import { HashLoader } from "react-spinners";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faEnvelope, faAnglesRight, faChevronDown, faChevronUp } from "@fortawesome/free-solid-svg-icons";
import styles from "./Verify.module.css";
import Chatbot from "./Chatbot";
import ClaimProcessingStatus from "../MainContent/ClaimProcessingStatus";
import VerificationDB from "./VerificationDB";
import Insights from "./Insights";

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

  // Collapsible states for panels
  const [isVerificationSummaryOpen, setVerificationSummaryOpen] = useState(true);
  const [isVerificationDBOpen, setVerificationDBOpen] = useState(true);

  const handleEmail = () => {
    navigate("/generate-email", {
      state: { Dsummary, summary, recommendation, recNum, psid },
    });
  };

  if (loading) {
    return (
      <div className={styles.spinnerContainer}>
        <HashLoader color="#0f5fdc" size={40} />
      </div>
    );
  }

  if (error) {
    return (
      <div className={styles.errorContainer}>
        <p>Error: {error}</p>
      </div>
    );
  }

  return (
    <div className={styles.verifyMainContainer}>
      {/* Header Section */}
      <div className={styles.headerSection}>
        <div className={styles.claimIdDisplay}>
          <div className={styles.claimIdBadge}>
            <span>Claim ID</span>
            <h3>{recNum || "N/A"}</h3>
          </div>
          <div className={styles.claimIdBadge}>
            <span>PS ID</span>
            <h3>{psid || "N/A"}</h3>
          </div>
        </div>
        <div className={styles.statusSection}>
          <ClaimProcessingStatus percentage={emptyKeysPercentage} isLoading={loading} />
        </div>
      </div>

      <div className={styles.verifyContainer}>
        {/* Chatbot */}
        <div className={styles.chatbotPanel}>
          <Chatbot />
        </div>

        {/* Main Content Panels */}
        <div className={styles.mainPanels}>
          <div className={styles.leftPanel}>
            {/* Claim Summary */}
            <div className={styles.claimSummary}>
              <div className={styles.panelHeader}>
                <h3>Claim Summary</h3>
              </div>
              <div className={styles.panelContent}>
                <p>{Dsummary || "No summary available"}</p>
              </div>
            </div>

            {/* Insights Panel */}
            <div className={styles.insightsPanel}>
              <div className={styles.panelHeader}>
                <h3>Insights From Historic Claims</h3>
              </div>
              <div className={styles.panelContent}>
                <Insights claimType={summary?.claimType || "GENERAL"} />
              </div>
            </div>
          </div>

          <div className={styles.rightPanel}>
            {/* Verification Summary Panel */}
            <div className={styles.verificationPanel}>
              <div
                className={styles.panelHeader}
                onClick={() => setVerificationSummaryOpen(!isVerificationSummaryOpen)}
              >
                <h3>Verification Summary</h3>
                <FontAwesomeIcon
                  icon={isVerificationSummaryOpen ? faChevronUp : faChevronDown}
                  className={styles.toggleIcon}
                />
              </div>
              <div
                className={`${styles.collapsibleContent} ${isVerificationSummaryOpen ? styles.open : styles.closed}`}
              >
                <div className={styles.summarySection}>
                  <h4>Detailed Recommendation</h4>
                  <p>{recommendation || "No detailed recommendation available"}</p>
                </div>
              </div>
            </div>

            {/* Verification With Integrated System Panel */}
            <div className={styles.verificationPanel}>
              <div
                className={styles.panelHeader}
                onClick={() => setVerificationDBOpen(!isVerificationDBOpen)}
              >
                <h3>Verification With Integrated System</h3>
                <FontAwesomeIcon
                  icon={isVerificationDBOpen ? faChevronUp : faChevronDown}
                  className={styles.toggleIcon}
                />
              </div>
              <div
                className={`${styles.collapsibleContent} ${isVerificationDBOpen ? styles.open : styles.closed}`}
              >
                <VerificationDB recNum={recNum} psid={psid} />
              </div>
            </div>
          </div>
        </div>
      </div>

      {/* Generate Email Button */}
      <div className={styles.generateEmailContainer}>
        <button
          className={styles.generateEmailButton}
          onClick={handleEmail}
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




/* General Styles for the Panels */
.verifyContainer {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 30px;
}

.leftPanel, .rightPanel {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 15px;
  height: 100%;  /* Ensure both panels take equal height */
  max-height: 100vh;  /* Prevents the layout from growing too tall */
}

.verificationPanel, .claimSummary, .insightsPanel, .chatbotPanel {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 15px;
  height: 100%;
  max-height: 100%; /* Ensure it fits within the panel's height */
}

/* Specific scroll behavior for collapsible sections */
.collapsibleContent {
  overflow-y: auto;  /* Enables scrolling when content overflows */
  max-height: calc(100% - 50px);  /* Adjust based on your header height */
}

/* Scrollbar styling */
.collapsibleContent::-webkit-scrollbar {
  width: 6px;
}

.collapsibleContent::-webkit-scrollbar-thumb {
  background: #888;
  border-radius: 10px;
}

.collapsibleContent::-webkit-scrollbar-thumb:hover {
  background: #555;
}

/* Equal section height and scrollable area */
.mainPanels {
  display: flex;
  flex-direction: row;
  gap: 30px;
  justify-content: space-between;
  height: 100%;
  max-height: 100vh;
}

.headerSection {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 30px;
  margin-bottom: 20px;
}

.panelHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
}

.panelHeader h3 {
  margin: 0;
}

/* Optional: Add a little padding and box-shadow to give the sections a clean look */
.verificationPanel, .claimSummary, .insightsPanel {
  padding: 15px;
  background-color: #f4f4f4;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.generateEmailContainer {
  margin-top: 20px;
  display: flex;
  justify-content: flex-end;
}

.generateEmailButton {
  background-color: #0f5fdc;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 8px;
  display: flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.generateEmailButton:hover {
  background-color: #08479d;
}

.generateEmailButton:disabled {
  background-color: #dcdcdc;
  cursor: not-allowed;
}
