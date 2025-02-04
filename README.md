add styling on the Insights and the Verification DB i want this will look like in inline and same width height like Claim Summary, Verification Summary :- import React from "react"; import { useNavigate } from "react-router-dom"; import { HashLoader } from "react-spinners"; import { FontAwesomeIcon } from "@fortawesome/react-fontawesome"; import { faEnvelope } from "@fortawesome/free-solid-svg-icons"; import styles from "./Verify.module.css"; import Chatbot from "./Chatbot"; import ClaimProcessingStatus from "../MainContent/ClaimProcessingStatus"; import VerificationDB from "./VerificationDB"; import Insights from "./Insights"; // Import the Insights component

const VerifyContent = ({ recNum, psid, Dsummary, summary, recommendation, loading, error, emptyKeysPercentage, }) => { const navigate = useNavigate();

const HandleEmail = () => { navigate("/generate-email", { state: { Dsummary, summary, recommendation, recNum, psid, }, }); };

// Render loading state if (loading) { return ( <div className={styles.spinnerContainer}> <HashLoader color="#0f5fdc" size={40} /> </div> ); }

// Render error state if (error) { return ( <div className={styles.errorContainer}> <p>Error: {error}</p> </div> ); }

// Render no data state if (!summary || !recommendation) { return ( <div className={styles.noDataContainer}> <p>No verification data available.</p> </div> ); }

return ( <div className={styles.verifyMainContainer}> {/* Claim ID Display */} <div className={styles.headerSection}> <div className={styles.claimIdDisplay}> <div className={styles.claimIdBadge}> <span>Claim ID</span> <h3>{recNum || 'N/A'}</h3> </div> <div className={styles.claimIdBadge}> <span>PS ID</span> <h3>{psid || 'N/A'}</h3> </div> </div>

    <div className={styles.statusSection}>
      <ClaimProcessingStatus 
        percentage={emptyKeysPercentage} 
        isLoading={loading} 
      />
    </div>
  </div>

  {/* Add ClaimProcessingStatus component */}
  

  <div className={styles.verifyContainer}>
    <div>
      <Chatbot/>
    </div>
    {/* Left Panel - Claim Summary */}
<div className={styles.rightLeft}> <div className={styles.leftPanel}> <div className={styles.panelHeader}> <h3>Claim Summary</h3> </div> <div className={styles.panelContent}> <p>{Dsummary || 'No summary available'}</p> </div> </div> {/* Right Panel - Verification Summary */} <div className={styles.rightPanel}> <div className={styles.panelHeader}> <h3>Verification Summary</h3> </div> <div className={styles.panelContent}> <div className={styles.summarySection}> <h4>Detailed Recommendation</h4> <p>{recommendation || 'No detailed recommendation available'}</p> </div>
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
    <div className={styles.insightsContainer}>
    <Insights 
      claimType={summary?.claimType || 'GENERAL'} 
    />
  </div>
          <VerificationDB 
    recNum={recNum} 
    psid={psid} 
  />

  </div>

  {/* Add Insights component */}
  

  {/* Generate Email Button */}
  <div className={styles.generateEmailContainer}>
    <button 
      className={styles.generateEmailButton} 
      onClick={HandleEmail}
      disabled={!summary || !recommendation}
    >
      <FontAwesomeIcon icon={faEnvelope} className={styles.emailIcon} />
      Generate Email
    </button>
  </div>
</div>
); };

export default VerifyContent;

.verifyMainContainer { max-width: 1400px; margin: 0 auto; padding: 20px; background-color: #f4f6f9; border-radius: 12px; }

/* Header Section */ .headerSection { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; gap: 20px; }

.claimIdDisplay { display: flex; gap: 20px; flex-grow: 1; }

.claimIdBadge { flex: 1; background-color: white; border-radius: 10px; padding: 15px; box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05); transition: transform 0.3s ease; }

.claimIdBadge:hover { transform: translateY(-5px); }

.claimIdBadge span { display: block; color: #6b7280; font-size: 0.9rem; margin-bottom: 8px; text-transform: uppercase; letter-spacing: 1px; }

.claimIdBadge h3 { margin: 0; color: #2c3e50; font-size: 1.2rem; font-weight: 600; }

.rightLeft{ display: flex; gap:10px; }

.leftPanel, .rightPanel { flex: 1; background-color: white; border-radius: 12px; border: 1px solid #e1e4e8; box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05); overflow: hidden; }

.panelHeader { background-color: #f1f5f9; padding: 15px 20px; }

.panelHeader h3 { margin: 0; color: #2c3e50; font-size: 1.1rem; }

.panelContent { padding: 20px; }

.summarySection h4 { color: #0f5fdc; margin-bottom: 10px; padding-bottom: 5px; }

.claimDetails { margin-top: 20px; background-color: #f9fafb; border-radius: 8px; padding: 15px; }

.claimDetailItem { display: flex; justify-content: space-between; margin-bottom: 10px; padding-bottom: 10px; border-bottom: 1px solid #e2e8f0; }

.claimDetailItem:last-child { border-bottom: none; }

.claimDetailItem strong { color: #2c3e50; }

.claimDetailItem span { color: #4a5568; }

.generateEmailContainer { display: flex; justify-content: center; margin-top: 20px; }

.generateEmailButton { display: flex; align-items: center; gap: 10px; padding: 12px 24px; background:linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc); color: white; border: none; border-radius: 8px; cursor: pointer; transition: all 0.3s ease; }

.generateEmailButton:hover { background-color: #1e40af; transform: translateY(-2px); }

.generateEmailButton:disabled { background-color: #a0aec0; cursor: not-allowed; }

.emailIcon { font-size: 1.2rem; }

/* Spinner */ .spinnerContainer { display: flex; justify-content: center; align-items: center; height: 100vh; }

/.claimProcessingStatusContainer {/ /* margin-bottom: 20px;/ / width: 100%;/ / display: flex;/ / justify-content: center;/ /}/ .claimProcessingStatusContainer { background-color: #ffffff; border-radius: 16px; box-shadow: 0 6px 12px rgba(0, 0, 0, 0.08); padding: 20px; display: flex; flex-direction: column; gap: 15px; transition: all 0.3s ease; max-width: 350px; /margin: 0 auto;/ } .statusSection{ / flex: 1 1; */ background-color: white; border-radius: 10px; padding: 10px; box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05); transition: transform 0.3s ease; }

.statusSection:hover { transform: translateY(-5px); } .insightsContainer { background-color: #ffffff; border-radius: 16px; box-shadow: 0 6px 12px rgba(0, 0, 0, 0.08); /padding: 20px;/ display: flex; flex-direction: column; gap: 15px; transition: all 0.3s ease; max-width: 350px; /margin: 0 auto;/ }
