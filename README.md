
import React, { useEffect, useState } from "react";
import { useLocation } from "react-router-dom";
import axios from "axios";
import styles from "./Verify.module.css";
import { useNavigate } from "react-router-dom";
import { HashLoader } from "react-spinners";


const Verify = () => {
  const location = useLocation();
  

  // Define state variables for storing the data, loading state, and error
  const [summary, setSummary] = useState(null);
  const [recommendation, setRecommendation] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
      const navigate = useNavigate();


const HandleEmail = () => {
  navigate("/generate-email", {
    state: {
      summary,
      recommendation,
    },
  });
};
  // Use effect to fetch data when component mounts
  useEffect(() => {
    const fetchData = async (recNum, psid) => {
      try {
        // Prepare your payload
        const payload = {
         tasktype: "VERIFY_CLAIM",
         claimid: "claimid",
         psid: "psid" ,
        };

        // Prepare custom headers (if needed)
        const headers = {
          "Content-Type": "application/json", // Adjust content type based on your needs

        };

        // Example API call with payload and headers
        const response = await axios.post(
          "https://e21wxu9skj.execute-api.us-east-1.amazonaws.com/dev/querequest", // Replace with your actual endpoint
          payload,
          { headers } // Pass headers as part of the request
        );
        
        console.log('Summary:', response.data)

      // Parse the response body (since it's a JSON string)
        const responseBody = JSON.parse(response.data.verifyclaimactdata.body);

        // Extract the summary details and recommendation
        const { CLAIM_FORM_BRIEF_SUMMARY, CLAIM_FORM_TYPE, CLAIM_STATUS, DETAILED_SUMMARY } = responseBody.SUMMARY_DETAILS;

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
      fetchData();
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
      {/* Left side: Summary */}
      <div className={styles.leftPanel}>
        <h3>Claim Summary</h3>
        <p><strong>Brief Summary:</strong> {summary.briefSummary}</p>
        <p><strong>Claim Type:</strong> {summary.claimType}</p>
        <p><strong>Claim Status:</strong> {summary.claimStatus}</p>
      </div>

      {/* Right side: Recommendations */}
      <div className={styles.rightPanel}>
        <h3>Detailed Summary</h3>
        <p>{recommendation}</p>
      </div>
      
      <div className={styles.genrateEmailContainer}>
          <button className={styles.generateEmailButton} onClick={HandleEmail}>
          Generate Email
          </button>
      </div>
    </div>
  );
};

export default Verify;
