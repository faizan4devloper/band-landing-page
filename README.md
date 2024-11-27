const handleVerify = (recNum) => {
  navigate("/verify", { state: { recNum } }); // Pass recNum in state
};



<div className={styles.verifyButtonContainer}>
  <button
    className={styles.verifyButton}
    onClick={() => handleVerify(data?.claimid)} // Replace `data?.claimid` with the actual recNum or identifier
  >
    Verify
  </button>
</div>




import React, { useEffect, useState } from "react";
import { useLocation, useNavigate } from "react-router-dom";
import axios from "axios";
import styles from "./Verify.module.css";
import { HashLoader } from "react-spinners";

const Verify = () => {
  const location = useLocation();
  const navigate = useNavigate();

  // State variables
  const [summary, setSummary] = useState(null);
  const [recommendation, setRecommendation] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  // Fetch data function
  const fetchData = async (recNum) => {
    try {
      const payload = {
        tasktype: "VERIFY_CLAIM",
        claimid: recNum,
        PSID: "PS391481",
      };

      const headers = {
        "Content-Type": "application/json",
      };

      const response = await axios.post("dummy", payload, { headers });
      console.log("Summary:", response.data);

      // Parse the response
      const responseBody = JSON.parse(response.data.verifyclaimactdata.body);
      const { CLAIM_FORM_BRIEF_SUMMARY, CLAIM_FORM_TYPE, CLAIM_STATUS, DETAILED_SUMMARY } = responseBody.SUMMARY_DETAILS;

      setSummary({
        briefSummary: CLAIM_FORM_BRIEF_SUMMARY,
        claimType: CLAIM_FORM_TYPE,
        claimStatus: CLAIM_STATUS,
      });
      setRecommendation(DETAILED_SUMMARY);
    } catch (error) {
      setError("Failed to load data");
      console.error(error);
    } finally {
      setLoading(false);
    }
  };

  // UseEffect to fetch data when `recNum` is available
  useEffect(() => {
    if (location.state?.recNum) {
      fetchData(location.state.recNum); // Use recNum from state
    } else {
      setError("No claim ID provided.");
      setLoading(false);
    }
  }, [location.state]);

  // Loading and error handling
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
        <p><strong>Brief Summary:</strong> {summary.briefSummary}</p>
        <p><strong>Claim Type:</strong> {summary.claimType}</p>
        <p><strong>Claim Status:</strong> {summary.claimStatus}</p>
      </div>
      <div className={styles.rightPanel}>
        <h3>Detailed Summary</h3>
        <p>{recommendation}</p>
      </div>
      <div className={styles.genrateEmailContainer}>
        <button className={styles.generateEmailButton} onClick={() => navigate("/generate-email")}>
          Generate Email
        </button>
      </div>
    </div>
  );
};

export default Verify;
