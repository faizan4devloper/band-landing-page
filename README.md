
import React, { useEffect, useState } from "react";
import { useLocation } from "react-router-dom";
import axios from "axios";
import styles from "./Verify.module.css";
import { useNavigate } from "react-router-dom";
import { HashLoader } from "react-spinners";


const Verify = ({rows}) => {
  const location = useLocation();
  const recNum = location.state?.recNum || "CL1234567"
  
    const Dsummary = location.state?.summary || "No Summary Available";


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
      recNum,
    },
  });
};
  // Use effect to fetch data when component mounts
  useEffect(() => {
    const fetchData = async () => {
      try {
        // Prepare your payload
        const payload = {
         tasktype: "VERIFY_CLAIM",
         claimid: recNum,
        // CL1234567
         psid: "PS391481" ,
        };

        // Prepare custom headers (if needed)
        const headers = {
          "Content-Type": "application/json", // Adjust content type based on your needs

        };

        // Example API call with payload and headers
        const response = await axios.post(
          "dummy", // Replace with your actual endpoint
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
    if (location.state) {
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
    <div>
      
          <div className={styles.claimIdDisplay}>
          <h3>Claim ID: {recNum}</h3>  
          </div>
    <div className={styles.verifyContainer}>
      {/* Left side: Summary */}
      <div className={styles.leftPanel}>
        <h3>Claim Summary</h3>
                  <p><strong>Summary:</strong> {Dsummary}</p>

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
        </div>

  );
};

export default Verify;



.verifyContainer {
  display: flex;
  justify-content: space-between;
  gap: 20px;
  padding: 20px;
}

.leftPanel, .rightPanel {
  width: 48%; /* Each panel takes up 48% of the screen */
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.leftPanel h3, .rightPanel h3 {
  margin-top: 0;
  color: #333;
}

.leftPanel p, .rightPanel p {
  font-size: 14px;
  line-height: 1.5;
  color: #555;
}

.genrateEmailContainer {
  margin-top: 20px;
  text-align: center;
}

.generateEmailButton {
     padding: 10px 20px;
    background:linear-gradient(90deg, #0f5fdc, #7ca2e1, #0f5fdc);
    color: #fff;
    border: none;
    border-radius: 5px;
    position: absolute;
    right: 45%;
    top: 84%;
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

.claimIdDisplay{
     margin: 0px 25px;
    font-size: 16px;
    margin-bottom: 0px;
    font-weight: bold;
    color: #333;
}
