i'm going to display the Insights and VerificationDB in the VerificationContent.js i want cosisitency cause after all content display the insights still loading its not makes any sence i want conisistency across all content and section with perfect :- 
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
            <span>Productsheet ID</span>
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
            <h3>Verification Detail</h3>
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
                <h3>Verification With System</h3>
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


.verifyMainContainer {
  max-width: 1400px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f4f6f9;
  border-radius: 12px;
}

/* Header Section */
.headerSection {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  gap: 20px;
}

.claimIdDisplay {
  display: flex;
  gap: 20px;
  flex-grow: 1;
}

.claimIdBadge {
  flex: 1;
  background-color: white;
  border-radius: 10px;
  padding: 15px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
  transition: transform 0.3s ease;
}

.claimIdBadge:hover {
  transform: translateY(-5px);
}

.claimIdBadge span {
  display: block;
  color: #6b7280;
  font-size: 0.9rem;
  margin-bottom: 8px;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.claimIdBadge h3 {
  margin: 0;
  color: #2c3e50;
  font-size: 1.2rem;
  font-weight: 600;
}

.mainPanels {
  display: flex;
  gap: 15px;
  
}

/* Left Panel (Unchanged) */
.leftPanel {
  flex: 1;
  display: flex;
  justify-content: center;
  flex-direction: column;
  gap: 15px;
}

/* Right Panel - Column Layout */
.rightPanel {
  flex: 1;
  display: flex;
  align-items: center;
  flex-direction: column;
  gap: 15px;
      background-color: white;
    border-radius: 12px;
    border: 1px solid #e1e4e8;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
    padding: 20px;
}

/* Collapsible Panels */
.verificationPanel {
   background-color: white;
  border-radius: 12px;
  border: 1px solid #e1e4e8;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
    padding: 20px;

}

.collapsibleContent {
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.4s ease-in-out, opacity 0.3s ease-in-out;
  opacity: 0;
}

.collapsibleContent.open {
  max-height: 500px;
  opacity: 1;
}

.collapsibleContent.closed {
  max-height: 0;
  opacity: 0;
}
.panelHeader {
  background-color: #f1f5f9;
  padding: 15px 20px;
}

.panelHeader h3 {
  margin: 0;
  color: #2c3e50;
  font-size: 1.1rem;
}



.claimSummary{
   background-color: white;
  border-radius: 12px;
  border: 1px solid #e1e4e8;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
  padding: 20px;
}

.summarySection h4 {
  color: #0f5fdc;
  margin-bottom: 10px;
  padding-bottom: 5px;
}

.claimDetails {
  margin-top: 20px;
  background-color: #f9fafb;
  border-radius: 8px;
  padding: 15px;
}

.claimDetailItem {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
  padding-bottom: 10px;
  border-bottom: 1px solid #e2e8f0;
}

.claimDetailItem:last-child {
  border-bottom: none;
}

.claimDetailItem strong {
  color: #2c3e50;
}

/*.claimDetailItem span {*/
/*  color: #4a5568;*/
/*}*/

/* Bottom Panel (Insights & VerificationDB) */
.bottomPanel {
  display: flex;
  gap: 10px;
  margin-top: 20px;
}

.insightsPanel, .verificationDBPanel {
  flex: 1;
  background-color: white;
  border-radius: 12px;
  border: 1px solid #e1e4e8;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
  overflow: hidden;
  /*padding: 20px;*/
}

.collapsibleContent {
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.4s ease-in-out, opacity 0.3s ease-in-out;
  opacity: 0;
}

.collapsibleContent.open {
  max-height: 500px; /* Adjust height based on content */
  opacity: 1;
}

.collapsibleContent.closed {
  max-height: 0;
  opacity: 0;
}
.toggleIcon {
  font-size: 1.2rem;
  color: #0f5fdc;
  transition: transform 0.3s ease;
}

/* Insights Panel */
.insightsPanel {
  background-color: white;
  border-radius: 12px;
  border: 1px solid #e1e4e8;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
  padding: 20px;
}
/* Toggle Icon */
.panelHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #f1f5f9;
  padding: 15px 20px;
  cursor: pointer;
  transition: background 0.3s ease;
}



.panelHeader:hover {
  background-color: #e1e4e8;
}

.toggleIcon {
  font-size: 1.2rem;
  color: #0f5fdc;
  transition: transform 0.3s ease;
}


.generateEmailContainer {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.generateEmailButton {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 12px 24px;
  background:linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.generateEmailButton:hover {
  background-color: #1e40af;
  transform: translateY(-2px);
}

.generateEmailButton:disabled {
  background-color: #a0aec0;
  cursor: not-allowed;
}

.emailIcon {
  font-size: 1.2rem;
}


/* Spinner */
.spinnerContainer {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

/*.claimProcessingStatusContainer {*/
/*  margin-bottom: 20px;*/
/*  width: 100%;*/
/*  display: flex;*/
/*  justify-content: center;*/
/*}*/
.claimProcessingStatusContainer {
  background-color: #ffffff;
  border-radius: 16px;
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.08);
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 15px;
  transition: all 0.3s ease;
  max-width: 350px;
  /*margin: 0 auto;*/
}
.statusSection{
          /* flex: 1 1; */
    background-color: white;
    border-radius: 10px;
    padding: 10px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
    transition: transform 0.3s ease;
}

.statusSection:hover {
  transform: translateY(-5px);
}

.badge {
  padding: 4px 8px;
  border-radius: 4px;
  font-weight: bold;
  text-transform: uppercase;
  font-size: 0.8em;
}

.badgeSuccess {
     background-color: #e6f7ff;
    color: #28a745;
    border-radius: 20px;
    background-color: #d4edda;
    color: #28a745;
    /* border: 1px solid #52c41a; */
}

.badgeDanger {
  background-color: #fff1f0;
  color: #f5222d;
  border: 1px solid #f5222d;
}

.badgeWarning {
  background-color: #fffbe6;
  color: #faad14;
  border: 1px solid #faad14;
}



import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './Insights.module.css';
import { BeatLoader } from 'react-spinners';

import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faLightbulb, faChartLine, faCheckCircle } from '@fortawesome/free-solid-svg-icons';

const Insights = ({ claimType }) => {
  const [historicData, setHistoricData] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchHistoricClaimDetails = async () => {
      if (!claimType) return;

      setLoading(true);
      try {
        const response = await axios.post(
          'https://xhb68prpah.execute-api.us-east-1.amazonaws.com/devstage/history',
          { claimtype: claimType }
        );

        // Log the raw response for debugging
        console.log('Raw Response:', response.data);

        // Try different parsing approaches
        let parsedBody;
        try {
          // First, try parsing as nested JSON
          parsedBody = JSON.parse(JSON.parse(response.data.body));
        } catch (nestedParseError) {
          try {
            // If nested parsing fails, try direct parsing
            parsedBody = JSON.parse(response.data.body);
          } catch (directParseError) {
            // If both fail, parse the body directly
            parsedBody = response.data.body;
          }
        }

        // Validate parsed data
        if (!parsedBody || typeof parsedBody !== 'object') {
          throw new Error('Invalid data format');
        }

        // Log parsed data for verification
        console.log('Parsed Historic Data:', parsedBody);

        setHistoricData(parsedBody);
        setLoading(false);
      } catch (err) {
        console.error('Error details:', {
          message: err.message,
          response: err.response ? err.response.data : 'No response',
          stack: err.stack
        });
        setError(err.message);
        setLoading(false);
      }
    };

    fetchHistoricClaimDetails();
  }, [claimType]);

  // Rest of the component remains the same...

  // Add more robust rendering with default values
  if (!historicData) {
    return (
      <div className={styles.noDataContainer}>
<BeatLoader 
              color="#0f5fdc" 
              // loading={loadingLLM} 
              size={15} 
            />      </div>
    );
  }

  return (
<div className={styles.insightsContainer}>

      {/* Approver Insights Section */}
      <div className={styles.insightsSection}>
        <div className={styles.insightsGrid}>
          {/* Approver 1 Focus */}
          <div className={styles.insightsCard}>
            <ul>
              {historicData.Approver1_Focuses_On?.map((focus, index) => (
                <li key={index}>{focus}</li>
              ))}
            </ul>
          </div>

          {/* Approver 2 Focus */}
          <div className={styles.insightsCard}>
            <ul>
              {historicData.Approver2_Focuses_On?.map((focus, index) => (
                <li key={index}>{focus}</li>
              ))}
            </ul>
          </div>
        </div>
      </div>

      {/* Additional Information Section */}
      <div className={styles.insightsSection}>
        <h4>Additional Information</h4>
        <ul>
          {historicData.Additional_Information?.map((info, index) => (
            <li key={index}>{info}</li>
          ))}
        </ul>
      </div>

      {/* Recommendations Section */}
     
    </div>  );
};

export default Insights;


.insightsContainer {
  /*background-color: #f9f9f9;*/
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.insightsSection {
  margin-bottom: 20px;
}

.insightsSection h3 {
  /*border-bottom: 2px solid #0f5fdc;*/
  padding-bottom: 10px;
  color: #333;
}

.insightsGrid {
  /*display: grid;*/
  grid-template-columns: 1fr 1fr;
  gap: 15px;
}

/*.insightsCard {*/
/*  background-color: white;*/
/*  border-radius: 6px;*/
/*  padding: 15px;*/
/*  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);*/
/*}*/

.insightsCard h4 {
  margin-bottom: 10px;
  color: #0f5fdc;
}

.insightsCard ul, 
.insightsSection ul {
  list-style-type: disc;
  padding-left: 20px;
}

.loadingSpinner,
.errorMessage,
.noDataMessage p{
  text-align: center;
  padding: 20px;
  /*color: #666;*/
}

.errorMessage {
  color: #dc3545;
}

.errorMsg{
  margin:auto 150px;
}

.noDataContainer{
    margin:250px;

}



import React, { useState, useEffect } from 'react';
import axios from 'axios';
import styles from './VerificationDB.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { 
  faCheckCircle, 
  faTimesCircle, 
  faExclamationTriangle 
} from '@fortawesome/free-solid-svg-icons';

const VerificationDB = ({ 
  recNum,  // Add recNum as a prop
  psid  // Add policyNumber as a prop
}) => {
  const [verificationData, setVerificationData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  // API Endpoint
  const API_ENDPOINT = 'https://xhb68prpah.execute-api.us-east-1.amazonaws.com/devstage/claimdbverify';

  // Fetch Verification Data
  const fetchVerificationData = async () => {
    try {
      setLoading(true);
      const payload = {
        claimid: recNum,
        recnumber: psid,  // You might want to pass this as a prop or use a default
        policynumber: "L2065777"
      };
      
      console.log('Payloads:', payload)

      const response = await axios.post(API_ENDPOINT, payload);
      console.log('VerificationDB:', response)
      setVerificationData(response.data);
      setLoading(false);
    } catch (error) {
      console.error('Verification DB Error:', error);
      setError(error.message || 'Failed to fetch verification data');
      setLoading(false);
    }
  };

  useEffect(() => {
    // Only fetch if recNum and policyNumber are provided
    if (recNum && psid) {
      fetchVerificationData();
    }
  }, [recNum, psid]);

  // Parse and categorize messages
  const parseMessages = (body) => {
    // Safely check if body exists and is a string
    if (!body || typeof body !== 'string') {
      return {
        successMessages: [],
        unsuccessMessages: []
      };
    }

    try {
      // More robust parsing method
      const successMatch = body.includes('Successful Messages:') 
        ? body.split('Successful Messages:')[1].split('Unsuccessful Messages:')[0]
        : '';
      
      const unsuccessMatch = body.includes('Unsuccessful Messages:')
        ? body.split('Unsuccessful Messages:')[1]
        : '';

      return {
        successMessages: successMatch
          .trim()
          .split('\n')
          .filter(msg => msg.trim() !== ''),
        unsuccessMessages: unsuccessMatch
          .trim()
          .split('\n')
          .filter(msg => msg.trim() !== '')
      };
    } catch (error) {
      console.error('Message parsing error:', error);
      return {
        successMessages: [],
        unsuccessMessages: []
      };
    }
  };

  // Render Message Section
const renderMessageSection = (messages, type) => {
  const icons = {
    success: faCheckCircle,
    error: faTimesCircle,
    warning: faExclamationTriangle
  };

  const messageStyles = {
    success: styles.successMessage,
    error: styles.errorMessage,
    warning: styles.warningMessage
  };

  return (
    <div className={messageStyles[`${type}Container`]}>
      <h4>
        <FontAwesomeIcon 
          icon={icons[type]} 
          className={styles[`${type}Icon`]} 
        />
        {type === 'success' ? 'Successful Verifications' : 'Issues Identified'}
      </h4>
      <ol className={styles.numberedMessageList}>
        {messages.map((message, index) => (
          <li 
            key={index} 
            className={`${styles.messageItem} ${messageStyles[type]}`}
          >
            <span className={styles.messageText}>{message}</span>
          </li>
        ))}
      </ol>
    </div>
  );
};
  // Detailed Rendering
  const renderVerificationContent = () => {
    // Loading State
    if (loading) return (
      <div className={styles.loadingContainer}>
        <div className={styles.spinner}></div>
        <p>Loading verification data...</p>
      </div>
    );

    // Error State
    if (error) return (
      <div className={styles.errorContainer}>
        <FontAwesomeIcon icon={faTimesCircle} className={styles.errorIcon} />
        <p>Error: {error}</p>
        <button onClick={fetchVerificationData}>Retry</button>
      </div>
    );

    // No Data State
    if (!verificationData || !verificationData.body) return (
      <div className={styles.noDataContainer}>
        <FontAwesomeIcon icon={faExclamationTriangle} className={styles.warningIcon} />
        <p>No verification data available</p>
      </div>
    );

    // Parse Messages
    const { successMessages, unsuccessMessages } = parseMessages(verificationData.body);

    return (
      <div className={styles.verificationContainer}>
       

        <div className={styles.messageContainer}>
          {successMessages.length > 0 && renderMessageSection(successMessages, 'success')}
          {unsuccessMessages.length > 0 && renderMessageSection(unsuccessMessages, 'error')}
        </div>
      </div>
    );
  };

  return (
    <div className={styles.verificationDBContainer}>
      {renderVerificationContent()}
    </div>
  );
};

export default VerificationDB;


.verificationDBContainer {
  /*background-color: #f8f9fa;*/
  border-radius: 12px;
  padding: 20px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.verificationContainer {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.verificationHeader {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 2px solid #e9ecef;
  padding-bottom: 15px;
}

.verificationHeader h2 {
  margin: 0;
  color: #2c3e50;
  font-size: 1.2rem;
}

.verificationSummary {
  display: flex;
  gap: 10px;
}

.successBadge, .errorBadge {
  padding: 5px 10px;
  border-radius: 20px;
  font-size: 0.8rem;
  font-weight: 600;
}

.successBadge {
  background-color: #d4edda;
  color: #28a745;
}

.errorBadge {
  background-color: #f8d7da;
  color: #dc3545;
}

.messageContainer {
  /*display: flex;*/
  gap: 20px;
}

.successMessageContainer, 
.errorMessageContainer {
  flex: 1;
  background-color: #ffffff;
  border-radius: 8px;
  padding: 15px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
}

.messageItem {
      /* margin-bottom: 10px; */
    padding: 8px;
    margin: 0;
    border-radius: 4px;
    font-size: 0.9rem;
}

.successMessage {
  /*background-color: #e6f3e6;*/
  /*color: #2e7d32;*/
}

.errorMessage {
  /*background-color: #f8d7da;*/
  /*color: #721c24;*/
}

.successIcon {
  color: #28a745;
  margin-right: 10px;
}

.errorIcon {
  color: #dc3545;
  margin-right: 10px;
}
