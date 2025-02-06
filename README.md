Uncaught runtime errors:
×
ERROR
(0 , react__WEBPACK_IMPORTED_MODULE_0__.useNavigate) is not a function
TypeError: (0 , react__WEBPACK_IMPORTED_MODULE_0__.useNavigate) is not a function
    at GenerateEmail (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:1836:70)
    at renderWithHooks (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:38396:22)
    at mountIndeterminateComponent (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:42367:17)
    at beginWork (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:43670:20)
    at HTMLUnknownElement.callCallback (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:28652:18)
    at Object.invokeGuardedCallbackDev (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:28696:20)
    at invokeGuardedCallback (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:28753:35)
    at beginWork$1 (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:48651:11)
    at performUnitOfWork (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:47899:16)
    at workLoopSync (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:47822:9)
ERROR
(0 , react__WEBPACK_IMPORTED_MODULE_0__.useNavigate) is not a function
TypeError: (0 , react__WEBPACK_IMPORTED_MODULE_0__.useNavigate) is not a function
    at GenerateEmail (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:1836:70)
    at renderWithHooks (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:38396:22)
    at mountIndeterminateComponent (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:42367:17)
    at beginWork (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:43670:20)
    at HTMLUnknownElement.callCallback (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:28652:18)
    at Object.invokeGuardedCallbackDev (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:28696:20)
    at invokeGuardedCallback (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:28753:35)
    at beginWork$1 (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:48651:11)
    at performUnitOfWork (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:47899:16)
    at workLoopSync (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:47822:9)
ERROR
(0 , react__WEBPACK_IMPORTED_MODULE_0__.useNavigate) is not a function
TypeError: (0 , react__WEBPACK_IMPORTED_MODULE_0__.useNavigate) is not a function
    at GenerateEmail (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:1836:70)
    at renderWithHooks (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:38396:22)
    at mountIndeterminateComponent (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:42367:17)
    at beginWork (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:43670:20)
    at beginWork$1 (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:48629:18)
    at performUnitOfWork (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:47899:16)
    at workLoopSync (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:47822:9)
    at renderRootSync (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:47795:11)
    at recoverFromConcurrentError (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:47287:24)
    at performSyncWorkOnRoot (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:47496:24)


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



import React, { useState, useEffect, useNavigate } from "react";
import { useLocation } from "react-router-dom";
import axios from "axios";
import styles from "./GenerateEmail.module.css";
import ClaimSummary from "./ClaimSummary";
import EmailGenerator from "./EmailGenerator";
import ApproverComments from "./ApproverComments";
import BreadCrumbs from "../../BreadCrumbs/BreadCrumbs";


const GenerateEmail = () => {
    const navigate = useNavigate();
      const breadcrumbItems = [
    { 
      label: 'Claims', 
      link: '/manage-claims' 
    },
    { 
      label: 'New Claim', 
      link: '/new-claim' 
    },
    { 
      label: 'Verify', 
      link: '/verify',
      // Explicitly pass the state when clicking on Verify breadcrumb
      onClick: () => {
        navigate('/verify', {
          state: {
            clid: recNum,
            selectedPolicy: psid,
            summary: Dsummary
          }
        });
      }
    },
    { label: 'Generate Email' }
  ];


  const location = useLocation();
    const [Dsummary, setDsummary] = useState(() => 
    location.state?.Dsummary || 
    localStorage.getItem('currentDsummary') || 
    null
  );
  
  const [summary, setSummary] = useState(() => 
    location.state?.summary || 
    localStorage.getItem('currentSummary') || 
    null
  );

  const [recommendation, setRecommendation] = useState(
    location.state?.recommendation || null
  );
  // const recNum = location.state?.recNum;
  // const psid = location.state?.psid;
  
    const recNum = location.state?.recNum || localStorage.getItem('currentClaimId');
  const psid = location.state?.psid || localStorage.getItem('currentPolicyId');

  
  const [loadingLLM, setLoadingLLM] = useState(true);
  const [errorLLM, setErrorLLM] = useState(null);
  const [llmResponse, setLlmResponse] = useState("");
  const [approverComments, setApproverComments] = useState("");

  useEffect(() => {
    const generateInitialLLMResponse = async () => {
      const payload = {
        claimid: recNum,
        recnumber: "PS12345",
      };
      
      setLoadingLLM(true);
      setErrorLLM(null);
      
      try {
        const response = await axios.post(
          "https://generateemail",
          payload,
          {
            headers: { "Content-Type": "application/json" },
          }
        );
        
        // Existing response parsing logic
        const extractedResponse = response.data.body || response.data;
        setLlmResponse(extractedResponse);
      } catch (err) {
        console.error("Error generating email:", err);
        setErrorLLM(err.message || "Failed to generate email");
      } finally {
        setLoadingLLM(false);
      }
    };

    generateInitialLLMResponse();
  }, [recNum]);
  
    useEffect(() => {
    if (Dsummary) localStorage.setItem('currentDsummary', Dsummary);
    if (summary) localStorage.setItem('currentSummary', summary);
    if (recommendation) localStorage.setItem('currentRecommendation', recommendation);
    if (recNum) localStorage.setItem('currentClaimId', recNum);
    if (psid) localStorage.setItem('currentPolicyId', psid);
  }, [Dsummary, summary, recommendation, recNum, psid]);


  const handleApproverCommentsChange = (e) => {
    setApproverComments(e.target.value);
  };

  return (
    <div className={styles.generateEmailMain}>
    <div className={styles.generateEmailMain}>
      <BreadCrumbs 
        items={breadcrumbItems} 
        onItemClick={(item) => {
          if (item.onClick) {
            item.onClick();
          } else if (item.link) {
            navigate(item.link);
          }
        }}
      />

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
      
      <div className={styles.container}>
        <div className={styles.leftSection}>
          <ClaimSummary 
            Dsummary={Dsummary} 
            recommendation={recommendation} 
            summary={summary}
          />
        </div>
        
        <div className={styles.rightSection}>
          <EmailGenerator 
            loadingLLM={loadingLLM}
            errorLLM={errorLLM}
            llmResponse={llmResponse}
          />
          
          <ApproverComments 
            approverComments={approverComments}
            handleApproverCommentsChange={handleApproverCommentsChange}
          />
        </div>
      </div>
    </div>
    </div>
  
  );

};


export default GenerateEmail;
