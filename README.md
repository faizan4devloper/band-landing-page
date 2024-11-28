const HandleEmail = () => {
  navigate("/generate-email", {
    state: {
      summary,
      recommendation,
    },
  });
};







import React, { useState, useEffect } from "react";
import { useLocation } from "react-router-dom";
import axios from "axios";
import styles from "./GenerateEmail.module.css";

const GenerateEmail = () => {
  const location = useLocation();
  const [summary, setSummary] = useState(location.state?.summary || null);
  const [recommendation, setRecommendation] = useState(
    location.state?.recommendation || null
  );
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
  }, []);

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

      console.log("LLM Response:", response.data);

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
          <p><strong>Brief Summary:</strong> {summary?.briefSummary}</p>
          <p><strong>Claim Type:</strong> {summary?.claimType}</p>
          <p><strong>Claim Status:</strong> {summary?.claimStatus}</p>
        </div>
        <div className={styles.sectionWindow}>
          <h2>Recommendations</h2>
          <p>{recommendation}</p>
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
