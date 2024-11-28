
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


const HandleEmail = ()=>{
    
 navigate("/generate-email")
}

  // Use effect to fetch data when component mounts
  useEffect(() => {
    const fetchData = async (recNum) => {
      try {
        // Prepare your payload
        const payload = {
         tasktype: "VERIFY_CLAIM",
         claimid: "CL557724",
         PSID: "PS485817" 
        };

        // Prepare custom headers (if needed)
        const headers = {
          "Content-Type": "application/json", // Adjust content type based on your needs

        };

        // Example API call with payload and headers
        const response = await axios.post(
          "", // Replace with your actual endpoint
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






import React, { useState, useEffect } from "react";
import axios from "axios";
import styles from "./GenerateEmail.module.css"; // Import CSS Module

const GenerateEmail = () => {
  const [loadingEmail, setLoadingEmail] = useState(false);
  const [loadingLLM, setLoadingLLM] = useState(false);
  const [errorEmail, setErrorEmail] = useState(null);
  const [errorLLM, setErrorLLM] = useState(null);
  const [parsedEmailBody, setParsedEmailBody] = useState("");
  const [llmResponse, setLlmResponse] = useState("");

  // Automatically fetch the email when the component mounts
  useEffect(() => {
    const fetchEmail = async () => {
      const payload = {
        claimid: "CL1234567",
        recnumber: "PS391481",
        tasktype: "FETCH_EMAIL",
      };

      setLoadingEmail(true);
      setErrorEmail(null);

      try {
        const response = await axios.post("", payload, {
          headers: { "Content-Type": "application/json" },
        });

        // Safely parse response
        const emailBodyString = response.data?.emailbody?.emailbody;
        if (emailBodyString) {
          setParsedEmailBody(emailBodyString.trim());
        } else {
          throw new Error("Invalid email body format");
        }
      } catch (err) {
        setErrorEmail("Failed to fetch email");
        console.error("Error:", err);
      } finally {
        setLoadingEmail(false);
      }
    };

    fetchEmail();
  }, []); // Runs only once on component mount

  // Handle LLM Response generation
  const handleGenerateLLMResponse = async () => {
    const payload = {
      claimid: "CL123456",
      recnumber: "PS391481",
      tasktype: "GENERATE_EMAIL",
    };

    setLoadingLLM(true);
    setErrorLLM(null);

    try {
      const response = await axios.post("", payload, {
        headers: { "Content-Type": "application/json" },
      });
      
      console.log("LLM Response:", response.data)

      // Safely parse response
      const llmResponseString = response.data?.generatedemaildata?.body;
      if (llmResponseString) {
        setLlmResponse(llmResponseString.trim());
      } else {
        throw new Error("Invalid LLM response format");
      }
    } catch (err) {
      setErrorLLM("Failed to generate LLM response");
      console.error("Error:", err);
    } finally {
      setLoadingLLM(false);
    }
  };

  return (
    <div className={styles.container}>
      <div className={styles.leftSection}>
        <div className={styles.sectionWindow}>
          <h2>Claim Summary</h2>
          <p>Details about the claim go here.</p>
        </div>
        <div className={styles.sectionWindow}>
          <h2>Recommendations</h2>
          <p>Suggestions based on the claim details go here.</p>
        </div>
      </div>
      <div className={styles.rightSection}>
        <div className={styles.sectionWindow}>
          <h2>Draft Email</h2>
          {loadingEmail && <p>Loading email content...</p>}
          {errorEmail && <p className={styles.errorText}>{errorEmail}</p>}
          {parsedEmailBody && (
            <div className={styles.emailPreview}>
              <h3>Email Body:</h3>
              <p className={styles.emailContent}>{parsedEmailBody}</p>
            </div>
          )}
        </div>
        <div className={styles.sectionWindow}>
          <h2>LLM Response</h2>
          <button
            className={styles.generateButton}
            onClick={handleGenerateLLMResponse}
            disabled={loadingLLM}
          >
            {loadingLLM ? "Generating LLM Response..." : "Generate LLM Response"}
          </button>
          {errorLLM && <p className={styles.errorText}>{errorLLM}</p>}
          {llmResponse && (
            <div className={styles.emailPreview}>
              <h3>LLM Response:</h3>
              <p className={styles.emailContent}>{llmResponse}</p>
            </div>
          )}
        </div>
        <button className={styles.submitButton}>Submit</button>
      </div>
    </div>
  );
};

export default GenerateEmail;
