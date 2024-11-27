import React, { useEffect, useState } from "react";
import { useLocation, useNavigate } from "react-router-dom";
import axios from "axios";
import styles from "./Verify.module.css";
import { HashLoader } from "react-spinners";

const Verify = () => {
  const location = useLocation();
  const navigate = useNavigate();

  // State variables for storing the data, loading state, and error
  const [summary, setSummary] = useState(null);
  const [recommendation, setRecommendation] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  // Handle the navigation to generate email page
  const handleEmail = () => {
    navigate("/generate-email");
  };

  // Fetch the data when the component mounts or when location.state changes
  useEffect(() => {
    const fetchData = async (recNum) => {
      try {
        // Prepare the payload for the API request
        const payload = {
          tasktype: "VERIFY_CLAIM",
          claimid: recNum,
          PSID: "PS391481", // Replace with actual PSID or retrieve it dynamically
        };

        // Custom headers for the API request (if needed)
        const headers = {
          "Content-Type": "application/json", // Adjust content type if needed
        };

        // Send the API request
        const response = await axios.post(
          "dummy", // Replace with actual endpoint URL
          payload,
          { headers }
        );

        // Log the response to verify the data structure
        console.log("API Response:", response.data);

        // Parse the response body (assuming it's a JSON string)
        const responseBody = JSON.parse(response.data.verifyclaimactdata.body);

        // Extract the required fields
        const {
          CLAIM_FORM_BRIEF_SUMMARY,
          CLAIM_FORM_TYPE,
          CLAIM_STATUS,
          CLAIM_FORM_DETAILED_SUMMARY, // Extract the detailed summary here
        } = responseBody.SUMMARY_DETAILS;

        // Set the state with the extracted data
        setSummary({
          briefSummary: CLAIM_FORM_BRIEF_SUMMARY,
          claimType: CLAIM_FORM_TYPE,
          claimStatus: CLAIM_STATUS,
        });
        setRecommendation(CLAIM_FORM_DETAILED_SUMMARY); // Store detailed summary as recommendation
      } catch (error) {
        setError("Failed to load data");
        console.error(error);
      } finally {
        setLoading(false);
      }
    };

    // If location.state exists, use it directly; otherwise, fetch the data
    if (!location.state) {
      fetchData("sampleRecNum"); // Replace with actual recNum or get it from location.state if needed
    } else {
      const { summary, recommendation } = location.state;
      setSummary(summary);
      setRecommendation(recommendation);
      setLoading(false);
    }
  }, [location.state]);

  // Show loading spinner or error message
  if (loading) {
    return (
      <div className={styles.spinnerContainer}>
        <HashLoader color="#0f5fdc" size={40} />
      </div>
    );
  }

  // Show error if the data couldn't be fetched
  if (error) {
    return <p>{error}</p>;
  }

  // Show message if summary or recommendation data is unavailable
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

      {/* Right side: Detailed Summary (Recommendation) */}
      <div className={styles.rightPanel}>
        <h3>Detailed Summary</h3>
        <p>{recommendation}</p> {/* This will display the CLAIM_FORM_DETAILED_SUMMARY */}
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
