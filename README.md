.verify-container {
  display: flex;
  justify-content: space-between;
  gap: 20px;
  padding: 20px;
}

.left-panel, .right-panel {
  width: 48%; /* Each panel takes up 48% of the screen */
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.left-panel h3, .right-panel h3 {
  margin-top: 0;
  color: #333;
}

.left-panel p, .right-panel p {
  font-size: 16px;
  line-height: 1.5;
  color: #555;
}

.generate-email-container {
  margin-top: 20px;
  text-align: center;
}

.generate-email-button {
  padding: 10px 20px;
  background-color: #007BFF;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s;
}

.generate-email-button:hover {
  background-color: #0056b3;
}
import React, { useEffect, useState } from "react";
import { useLocation } from "react-router-dom";
import axios from "axios";

const Verify = () => {
  const location = useLocation();

  // Define state variables for storing the data, loading state, and error
  const [summary, setSummary] = useState(null);
  const [recommendation, setRecommendation] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  // Use effect to fetch data when component mounts
  useEffect(() => {
    const fetchData = async () => {
      try {
        // Prepare your payload
        const payload = {
          claimId: "12345", // You can dynamically change this based on your requirement
        };

        // Prepare custom headers (if needed)
        const headers = {
          "Content-Type": "application/json", // Adjust content type based on your needs
          "Authorization": "Bearer YOUR_API_TOKEN", // Use your token or authentication details
          "Custom-Header": "Header-Value", // Add other headers if necessary
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
    return <p>Loading...</p>;
  }

  if (error) {
    return <p>{error}</p>;
  }

  // Check if summary or recommendation is not available and handle it
  if (!summary || !recommendation) {
    return <p>No data available.</p>;
  }

  return (
    <div>
      {/* Left side: Summary */}
      <div>
        <h3>Claim Summary</h3>
        <p><strong>Brief Summary:</strong> {summary.briefSummary}</p>
        <p><strong>Claim Type:</strong> {summary.claimType}</p>
        <p><strong>Claim Status:</strong> {summary.claimStatus}</p>
      </div>

      {/* Right side: Recommendations */}
      <div>
        <h3>Detailed Summary</h3>
        <p>{recommendation}</p>
      </div>
    </div>
  );
};

export default Verify;
