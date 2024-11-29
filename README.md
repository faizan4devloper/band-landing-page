
import React, { useEffect, useState } from "react";
import { useLocation } from "react-router-dom";
import axios from "axios";
import styles from "./Verify.module.css";
import { useNavigate } from "react-router-dom";
import { HashLoader } from "react-spinners";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faEnvelope } from "@fortawesome/free-solid-svg-icons";



const Verify = ({rows}) => {
  const location = useLocation();
  const recNum = location.state?.clid;
  console.log("inside verify.js recnumber:",recNum)
  console.log("inside verify.js rows is:",rows)
  
    const Dsummary = location.state?.summary || "No Summary Available";


  // Define state variables for storing the data, loading state, and error
  const [summary, setSummary] = useState(null);
  const [recommendation, setRecommendation] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
      const navigate = useNavigate();


const HandleEmail = () => {
  console.log("inside handle email recnumber:", recNum)
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
    {/* Claim ID Display */}
    <div className={styles.claimIdDisplay}>
      <h3>Claim ID: {recNum}</h3>
    </div>

    <div className={styles.verifyContainer}>
      {/* Left Panel */}
      <div className={styles.leftPanel}>
        <h3>Claim Summary</h3>
        <p><strong>Summary:</strong> {Dsummary}</p>
        <p><strong>Claim Type:</strong> {summary.claimType}</p>
        <p><strong>Claim Status:</strong> {summary.claimStatus}</p>
      </div>

      {/* Right Panel */}
      <div className={styles.rightPanel}>
        <h3>Detailed Summary</h3>
        <p>{recommendation}</p>
      </div>
    </div>

    {/* Generate Email Button */}
    <div className={styles.genrateEmailContainer}>
      <button className={styles.generateEmailButton} onClick={HandleEmail}>
        <FontAwesomeIcon icon={faEnvelope} />
        Generate Email
      </button>
    </div>
  </div>
);}

export default Verify;
