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
            percentage: emptyKeysPercentage, // ✅ Ensure percentage is passed back
          },
        });
      },
    },
    { label: "Generate Email" },
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
            <ClaimSummary Dsummary={Dsummary} recommendation={recommendation} summary={summary} />
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

