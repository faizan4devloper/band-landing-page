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
      link: '/claims' 
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
  
  const [emptyKeysPercentage, setEmptyKeysPercentage] = useState(()=>
  location.state?.percentage ?? localStorage.getItem('emptyKeysPercentage') ?? 100
  )
  
  useEffect(()=> {
    if(emptyKeysPercentage !== null){
      localStorage.setItem("emptyKeysPercentage", emptyKeysPercentage)
    }
  })

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
const { summary, recommendation, loading, error } = useVerifyData(
  recNum || localStorage.getItem("currentClaimId")
);
  // Clear storage when component unmounts
 useEffect(() => {
  return () => {
    // Only clear storage when navigating away from the claim flow
    if (!location.pathname.includes("/verify")) {
      localStorage.removeItem("currentClaimId");
      localStorage.removeItem("currentPolicyId");
      localStorage.removeItem("currentSummary");
    }
  };
}, [location.pathname]);
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
        emptyKeysPercentage={emptyKeysPercentage}
      />
    </div>
  );
};

export default Verify;




import React, { useState, useEffect } from "react";
import { useLocation, useNavigate } from "react-router-dom";
import axios from "axios";
import styles from "./GenerateEmail.module.css";
import ClaimSummary from "./ClaimSummary";
import EmailGenerator from "./EmailGenerator";
import ApproverComments from "./ApproverComments";
import BreadCrumbs from "../../BreadCrumbs/BreadCrumbs";

const GenerateEmail = () => {
  const navigate = useNavigate();
  const location = useLocation();

  // Retrieve state or localStorage values
  const [Dsummary, setDsummary] = useState(() =>
    location.state?.Dsummary || localStorage.getItem("currentDsummary") || null
  );

  const [summary, setSummary] = useState(() =>
    location.state?.summary || localStorage.getItem("currentSummary") || null
  );

  const [recommendation, setRecommendation] = useState(() =>
    location.state?.recommendation || localStorage.getItem("currentRecommendation") || null
  );

  const recNum = location.state?.recNum || localStorage.getItem("currentClaimId");
  const psid = location.state?.psid || localStorage.getItem("currentPolicyId");

  // Retrieve and persist emptyKeysPercentage
  const [emptyKeysPercentage, setEmptyKeysPercentage] = useState(() =>
    location.state?.emptyKeysPercentage ?? localStorage.getItem("emptyKeysPercentage") ?? 100
  );

  useEffect(() => {
    if (emptyKeysPercentage !== null) {
      localStorage.setItem("emptyKeysPercentage", emptyKeysPercentage);
    }
  }, [emptyKeysPercentage]);

  // Breadcrumb navigation
  const breadcrumbItems = [
    { label: "Claims", link: "/claims" },
    { label: "New Claim", link: "/new-claim" },
{
  label: "Verify",
  link: "/verify",
  onClick: () => {
    navigate("/verify", {
      state: {
        clid: recNum,
        selectedPolicy: psid,
        summary: Dsummary,
        recommendation: recommendation,
        percentage: emptyKeysPercentage, 
      },
    });
  },
},    { label: "Generate Email" },
  ];

  // AI Email Generation Request
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
    if (Dsummary) localStorage.setItem("currentDsummary", Dsummary);
    if (summary) localStorage.setItem("currentSummary", summary);
    if (recommendation) localStorage.setItem("currentRecommendation", recommendation);
    if (recNum) localStorage.setItem("currentClaimId", recNum);
    if (psid) localStorage.setItem("currentPolicyId", psid);
  }, [Dsummary, summary, recommendation, recNum, psid]);

  const handleApproverCommentsChange = (e) => {
    setApproverComments(e.target.value);
  };

  return (
    <div>
      {/* Breadcrumbs */}
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

      <div className={styles.generateEmailMain}>
        {/* Claim ID Display */}
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

        {/* Main Container */}
        <div className={styles.container}>
          {/* Left Section - Claim Summary */}
         <div className={styles.leftSection}>
        <div className={styles.sectionWindow}>
          <h2 classNam={styles.sectionWindowHead}>Claim Summary</h2>
          <p>{Dsummary}</p>
         
        </div>
        <div className={styles.sectionWindow}>
          <h2 classNam={styles.sectionWindowHead}>Verification Summary</h2>
          <p>{recommendation}</p>
           
        </div>
      </div>

          {/* Right Section - Email Generator & Approver Comments */}
          <div className={styles.rightSection}>
            <EmailGenerator loadingLLM={loadingLLM} errorLLM={errorLLM} llmResponse={llmResponse} />
            <ApproverComments approverComments={approverComments} handleApproverCommentsChange={handleApproverCommentsChange} />
          </div>
        </div>
      </div>
    </div>
  );
};

export default GenerateEmail;


import React, { useState } from "react";
import { useNavigate } from "react-router-dom";
import { HashLoader } from "react-spinners";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faEnvelope, faAnglesRight, faChevronDown, faChevronUp } from "@fortawesome/free-solid-svg-icons";
import styles from "./Verify.module.css";
import Chatbot from "./Chatbot";
// import ClaimProcessingStatus from "../MainContent/ClaimProcessingStatus";
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
                <h3>Against Productsheet</h3>
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
                <h3>Against system</h3>
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
          // disabled={!summary || !recommendation}
        >
          <FontAwesomeIcon icon={faEnvelope} className={styles.emailIcon} />
          Generate Email <FontAwesomeIcon icon={faAnglesRight} />
        </button>
      </div>
    </div>
  );
};

export default VerifyContent;


import { useState, useEffect } from "react";
import axios from "axios";

const useVerifyData = (recNum) => {
  const [summary, setSummary] = useState(null);
  const [recommendation, setRecommendation] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        const payload = {
          claimid: recNum,
          recnumber: "PS12345", 
        };

        const headers = {
          "Content-Type": "application/json",
        };

        const response = await axios.post(
          "https://verify", 
          payload,
          { headers }
        );
        
        console.log('Full API Response:', response);

        let responseBody;
        try {
          if (response.data.verifyclaimactdata && response.data.verifyclaimactdata.body) {
            responseBody = JSON.parse(response.data.verifyclaimactdata.body);
          } else if (response.data.body) {
            responseBody = JSON.parse(response.data.body);
          } else if (typeof response.data === 'string') {
            responseBody = JSON.parse(response.data);
          } else {
            responseBody = response.data;
          }

          console.log('Parsed Response Body:', responseBody);

          const summaryDetails = responseBody.SUMMARY_DETAILS || {};
          const {
            CLAIM_FORM_BRIEF_SUMMARY = 'No Brief Summary',
            CLAIM_FORM_TYPE = 'Unknown Type',
            CLAIM_STATUS = 'Pending',
            DETAILED_SUMMARY = 'No Detailed Summary'
          } = summaryDetails;

          setSummary({
            briefSummary: CLAIM_FORM_BRIEF_SUMMARY,
            claimType: CLAIM_FORM_TYPE,
            claimStatus: CLAIM_STATUS,
          });
          setRecommendation(DETAILED_SUMMARY);

        } catch (parseError) {
          console.error('Error parsing response:', parseError);
          setError('Failed to parse response data');
        }
      } catch (error) {
        console.error('API Error:', error);
        setError(error.response?.data?.message || 'Failed to load data');
      } finally {
        setLoading(false);
      }
    };

    if (recNum) {
      fetchData();
    } else {
      setError('No claim ID provided');
      setLoading(false);
    }
  }, [recNum]);

  return { summary, recommendation, loading, error };
};

export default useVerifyData;
