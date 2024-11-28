import React, { useEffect, useState } from "react";
import { useLocation, useNavigate } from "react-router-dom";
import axios from "axios";
import styles from "./Verify.module.css";
import { HashLoader } from "react-spinners";

const Verify = () => {
  const location = useLocation();
  const navigate = useNavigate();
  const recNum = location.state?.recNum || "CL1234567";

  const [summary, setSummary] = useState(null);
  const [recommendation, setRecommendation] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  const handleEmail = () => {
    navigate("/generate-email", {
      state: {
        summary,
        recommendation,
        recNum,
      },
    });
  };

  useEffect(() => {
    const fetchData = async () => {
      try {
        const payload = {
          tasktype: "VERIFY_CLAIM",
          claimid: recNum,
          psid: "PS391481",
        };

        const response = await axios.post("dummy", payload, {
          headers: { "Content-Type": "application/json" },
        });

        const responseBody = JSON.parse(response.data.verifyclaimactdata.body);

        const { CLAIM_FORM_BRIEF_SUMMARY, CLAIM_FORM_TYPE, CLAIM_STATUS, DETAILED_SUMMARY } =
          responseBody.SUMMARY_DETAILS;

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

    if (location.state?.summary) {
      setSummary({ briefSummary: location.state.summary });
      setLoading(false);
    } else {
      fetchData();
    }
  }, [location.state, recNum]);

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
    <div>
      <div className={styles.claimIdDisplay}>
        <h3>Claim ID: {recNum}</h3>
      </div>
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
          <button className={styles.generateEmailButton} onClick={handleEmail}>
            Generate Email
          </button>
        </div>
      </div>
    </div>
  );
};

export default Verify;
