import React, { useEffect, useState } from "react";
import { useLocation } from "react-router-dom";
import axios from "axios";
import ClipLoader from "react-spinners/ClipLoader"; // Import the spinner
import styles from "./Verify.module.css"; // Import the CSS module

const Verify = () => {
  const location = useLocation();

  // Define state variables for storing the data, loading state, and error
  const [claimId, setClaimId] = useState(null);
  const [summary, setSummary] = useState(null);
  const [recommendation, setRecommendation] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  // Use effect to fetch data when component mounts
  useEffect(() => {
    const fetchData = async () => {
      try {
        // Prepare your payload with the claim ID
        const payload = {
          claimId: "12345", // You can dynamically change this based on your requirement
        };

        // Prepare custom headers (if needed)
        const headers = {
          "Content-Type": "application/json", // Adjust content type based on your needs
          "Authorization": "Bearer YOUR_API_TOKEN", // Use your token or authentication details
        };

        // Example API call with payload and headers
        const response = await axios.post(
          "https://your-api-endpoint.com/verify-claim", // Replace with your actual endpoint
          payload,
          { headers } // Pass headers as part of the request
        );

        // Parse the response body (since it's a JSON string)
        const responseBody = JSON.parse(response.data.verifyclaimactdata.body);

        // Extract the summary details and recommendation
        const { CLAIM_FORM_BRIEF_SUMMARY, CLAIM_FORM_TYPE, CLAIM_STATUS, DETAILED_SUMMARY } = responseBody.SUMMARY_DETAILS;

        // Set the state variables with parsed data
        setClaimId(payload.claimId); // Set the Claim ID dynamically
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
      const { summary, recommendation, claimId } = location.state;
      setClaimId(claimId); // Get Claim ID from location state
      setSummary(summary);
      setRecommendation(recommendation);
      setLoading(false);
    }
  }, [location.state]);

  // Show loading or error message while fetching data
  if (loading) {
    return (
      <div className={styles.spinnerContainer}>
        <ClipLoader color="#007bff" size={50} />
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

  const handleGenerateEmail = () => {
    // Logic for generating email can go here (e.g., sending email or preparing content)
    console.log("Generate Email clicked");
  };

  return (
    <div className={styles.verifyContainer}>
      {/* Display the Claim ID at the top */}
      <div className={styles.claimIdContainer}>
        Claim ID: {claimId}
      </div>

      <div className={styles.leftRightContainer}>
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
      </div>

      <div className={styles.generateEmailContainer}>
        <button className={styles.generateEmailButton} onClick={handleGenerateEmail}>
          Generate Email
        </button>
      </div>
    </div>
  );
};

export default Verify;



.verifyContainer {
  padding: 20px;
}

.claimIdContainer {
  background-color: #007bff; /* Blue background for the Claim ID section */
  color: white; /* White text color */
  padding: 15px;
  text-align: center;
  font-size: 24px; /* Large font size for the Claim ID */
  font-weight: bold;
  border-radius: 8px;
  margin-bottom: 20px; /* Space below the Claim ID section */
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1); /* Soft shadow for depth */
}

.leftRightContainer {
  display: flex;
  justify-content: space-between;
  gap: 20px;
  padding-bottom: 20px;
}

.leftPanel,
.rightPanel {
  width: 48%;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.leftPanel h3,
.rightPanel h3 {
  margin-top: 0;
  color: #333;
}

.leftPanel p,
.rightPanel p {
  font-size: 16px;
  line-height: 1.5;
  color: #555;
}

.generateEmailContainer {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.generateEmailButton {
  padding: 10px 20px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s;
}

.generateEmailButton:hover {
  background-color: #0056b3;
}

/* Styling for the loading spinner */
.spinnerContainer {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh; /* Make the spinner cover the full viewport */
}
