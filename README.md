import React, { useEffect, useState } from "react";
import { useLocation, useNavigate } from "react-router-dom";
import axios from "axios";
import { HashLoader } from "react-spinners";
import styles from "./Verify.module.css";

const Verify = ({ message, rows, setRows, staticPreviewUrl }) => {
  const location = useLocation();
  const navigate = useNavigate();

  // State variables
  const [summary, setSummary] = useState(null);
  const [recommendation, setRecommendation] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  // Navigate to the email generation page
  const handleGenerateEmail = () => {
    navigate("/generate-email", {
      state: {
        summary,
        recommendation,
      },
    });
  };

  // Fetch data for claim verification
  useEffect(() => {
    const fetchData = async () => {
      try {
        const recNum = rows?.[0]?.recNum || location?.state?.recNum;
        if (!recNum) {
          throw new Error("Claim ID is missing!");
        }

        const payload = {
          tasktype: "VERIFY_CLAIM",
          claimid: recNum,
        };

        const headers = {
          "Content-Type": "application/json",
        };

        const response = await axios.post(
          "dummy", // Replace with your actual endpoint
          payload,
          { headers }
        );

        console.log("Verify API Response:", response.data);

        // Parse and extract data
        const responseBody = JSON.parse(response.data.verifyclaimactdata.body);
        const {
          CLAIM_FORM_BRIEF_SUMMARY,
          CLAIM_FORM_TYPE,
          CLAIM_STATUS,
          DETAILED_SUMMARY,
        } = responseBody.SUMMARY_DETAILS;

        setSummary({
          briefSummary: CLAIM_FORM_BRIEF_SUMMARY,
          claimType: CLAIM_FORM_TYPE,
          claimStatus: CLAIM_STATUS,
        });

        setRecommendation(DETAILED_SUMMARY);
      } catch (error) {
        console.error("Error fetching verification data:", error);
        setError("Failed to load claim verification data.");
      } finally {
        setLoading(false);
      }
    };

    if (!location.state || !location.state.summary || !location.state.recommendation) {
      fetchData();
    } else {
      const { summary, recommendation } = location.state;
      setSummary(summary);
      setRecommendation(recommendation);
      setLoading(false);
    }
  }, [location.state, rows]);

  if (loading) {
    return (
      <div className={styles.spinnerContainer}>
        <HashLoader color="#0f5fdc" size={40} />
      </div>
    );
  }

  if (error) {
    return <p className={styles.errorMessage}>{error}</p>;
  }

  if (!summary || !recommendation) {
    return <p className={styles.noDataMessage}>No data available.</p>;
  }

  return (
    <div className={styles.verifyContainer}>
      {/* Left panel: Claim summary */}
      <div className={styles.leftPanel}>
        <h3>Claim Summary</h3>
        <p>
          <strong>Brief Summary:</strong> {summary.briefSummary}
        </p>
        <p>
          <strong>Claim Type:</strong> {summary.claimType}
        </p>
        <p>
          <strong>Claim Status:</strong> {summary.claimStatus}
        </p>
      </div>

      {/* Right panel: Detailed recommendation */}
      <div className={styles.rightPanel}>
        <h3>Detailed Summary</h3>
        <p>{recommendation}</p>
      </div>

      {/* Generate Email Button */}
      <div className={styles.generateEmailContainer}>
        <button
          className={styles.generateEmailButton}
          onClick={handleGenerateEmail}
        >
          Generate Email
        </button>
      </div>
    </div>
  );
};

export default Verify;
