import React, { useEffect, useState } from "react";
import { useLocation, useNavigate } from "react-router-dom";
import axios from "axios";
import styles from "./Verify.module.css";
import { HashLoader } from "react-spinners";

const Verify = () => {
  const location = useLocation();
  const navigate = useNavigate();

  // Define state variables for storing the data, loading state, and error
  const [summary, setSummary] = useState(null);
  const [recommendation, setRecommendation] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  // Handle the action of navigating to generate email page
  const handleEmail = () => {
    navigate("/generate-email");
  };

  // Fetch data when component mounts or location.state changes
  useEffect(() => {
    const fetchData = async (recNum) => {
      try {
        // Prepare your payload
        const payload = {
          tasktype: "VERIFY_CLAIM",
          claimid: recNum,
          PSID: "PS391481", // Replace with actual PSID or retrieve from state
        };

        // Custom headers for API request
        const headers = {
          "Content-Type": "application/json", // Adjust content type based on your needs
        };

        // API call to fetch data
        const response = await axios.post(
          "dummy", // Replace with your actual endpoint
          payload,
          { headers } // Pass headers as part of the request
        );

        console.log("Summary:", response.data);

        // Parse the response body (assuming it's a JSON string)
        const responseBody = JSON.parse(response.data.verifyclaimactdata.body);

        // Extract the summary details and recommendation
        const {
          CLAIM_FORM_BRIEF_SUMMARY,
          CLAIM_FORM_TYPE,
          CLAIM_STATUS,
          DETAILED_SUMMARY,
        } = responseBody.SUMMARY_DETAILS;

        // Set the state variables with parsed data
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

    // Check if location.state exists, otherwise fetch the data
    if (!location.state) {
      fetchData("sampleRecNum"); // Replace with actual recNum or pass from state if needed
    } else {
      const { summary, recommendation } = location.state;
      setSummary(summary);
      setRecommendation(recommendation);
      setLoading(false);
    }
  }, [location.state]);

  // Show loading or error message while fetching data
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

  // Check if summary or recommendation is not available and handle it
  if (!summary || !recommendation) {
    return <p>No data available.</p>;
  }

  return (
    <div className={styles.verifyContainer}>
      {/* Left side: Claim Summary */}
      <div className={styles.leftPanel}>
        <h3>Claim Summary</h3>
        <p>
          <strong>Claim Type:</strong> {summary.claimType}
        </p>
        <p>
          <strong>Claim Status:</strong> {summary.claimStatus}
        </p>
      </div>

      {/* Right side: Detailed Summary */}
      <div className={styles.rightPanel}>
        <h3>Detailed Summary</h3>
        <p>{recommendation}</p>
      </div>

      {/* Generate Email Button */}
      <div className={styles.generateEmailContainer}>
        <button className={styles.generateEmailButton} onClick={handleEmail}>
          Generate Email
        </button>
      </div>
    </div>
  );
};

export default Verify;
