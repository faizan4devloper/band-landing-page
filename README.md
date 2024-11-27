const handleVerify = () => {
  const claimSummary = data?.CLAIM_FORM_DETAILS?.CLAIM_FORM_DETAILED_SUMMARY;

  // Pass the claim summary to Verify page through state
  navigate("/verify", {
    state: {
      summary: {
        claimType: data?.CLAIM_FORM_DETAILS?.CLAIM_FORM_TYPE,
        claimStatus: data?.CLAIM_STATUS,
      },
      recommendation: claimSummary, // Pass the detailed summary here
    },
  });
};





const handleVerify = () => {
  const claimSummary = data?.CLAIM_FORM_DETAILS?.CLAIM_FORM_DETAILED_SUMMARY;

  // Pass the claim summary to Verify page through state
  navigate("/verify", {
    state: {
      summary: {
        claimType: data?.CLAIM_FORM_DETAILS?.CLAIM_FORM_TYPE,
        claimStatus: data?.CLAIM_STATUS,
      },
      recommendation: claimSummary, // Pass the detailed summary here
    },
  });
};







import React, { useEffect, useState } from "react";
import { useLocation, useNavigate } from "react-router-dom";
import axios from "axios";
import styles from "./Verify.module.css";
import { HashLoader } from "react-spinners";

const Verify = () => {
  const location = useLocation();
  const navigate = useNavigate();

  const [summary, setSummary] = useState(null);
  const [recommendation, setRecommendation] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  const handleEmail = () => {
    navigate("/generate-email");
  };

  useEffect(() => {
    // Check if location.state is passed, if so, use it
    if (location.state) {
      const { summary, recommendation } = location.state;
      setSummary(summary); // This should contain claim summary data
      setRecommendation(recommendation); // This should contain detailed summary
      setLoading(false); // Once data is set, stop loading
    } else {
      // Fetch data if no state is passed
      const fetchData = async () => {
        try {
          const payload = {
            tasktype: "VERIFY_CLAIM",
            claimid: "sampleRecNum", // or pass the actual RecNum if available
            PSID: "PS391481",
          };
          const response = await axios.post("dummy", payload, { headers: { "Content-Type": "application/json" } });
          
          // Assuming you can extract CLAIM_FORM_DETAILED_SUMMARY here
          const responseBody = JSON.parse(response.data.verifyclaimactdata.body);
          const { CLAIM_FORM_DETAILED_SUMMARY } = responseBody.SUMMARY_DETAILS;

          setRecommendation(CLAIM_FORM_DETAILED_SUMMARY);
        } catch (error) {
          setError("Failed to load data");
          console.error(error);
        } finally {
          setLoading(false);
        }
      };

      fetchData();
    }
  }, [location.state]); // Only run when location.state changes

  if (loading) {
    return (
      <div className={styles.spinnerContainer}>
        <HashLoader color="#0f5fdc" size={40} />
      </div>
    );
  }

  if (error) {
    return <p>{error}</p>;
  }

  if (!summary || !recommendation) {
    return <p>No data available.</p>;
  }

  return (
    <div className={styles.verifyContainer}>
      <div className={styles.leftPanel}>
        <h3>Claim Summary</h3>
        <p><strong>Claim Type:</strong> {summary.claimType}</p>
        <p><strong>Claim Status:</strong> {summary.claimStatus}</p>
      </div>

      <div className={styles.rightPanel}>
        <h3>Detailed Summary</h3>
        <p>{recommendation}</p>
      </div>

      <div className={styles.generateEmailContainer}>
        <button className={styles.generateEmailButton} onClick={handleEmail}>
          Generate Email
        </button>
      </div>
    </div>
  );
};

export default Verify;
