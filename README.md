import React, { useState } from "react";
import axios from "axios";
import styles from "./GenerateEmail.module.css";

const GenerateEmail = () => {
  const [loading, setLoading] = useState(false);
  const [draftLoading, setDraftLoading] = useState(false);
  const [error, setError] = useState(null);
  const [draftError, setDraftError] = useState(null);
  const [responseData, setResponseData] = useState(null);
  const [draftEmail, setDraftEmail] = useState("");  // Updated state to store draft email content

  // API payloads
  const draftPayload = {
    claimid : "CL1234567", 
    recnumber: "PS391481",
    tasktype : "FETCH_EMAIL",
  };

  const generatePayload = {
    claimid: "CL123456",
    recnumber: "PS391481",
    tasktype: "GENERATE_EMAIL",
  };

  // Fetch draft email
  const handleFetchDraftEmail = async () => {
    setDraftLoading(true);
    setDraftError(null);

    try {
      const Emailresponse = await axios.post("", draftPayload, {
        headers: { "Content-Type": "application/json" },
      });

      // Extract emailbody from the response and update the state
      if (Emailresponse.data && Emailresponse.data.emailbody) {
        setDraftEmail(Emailresponse.data.emailbody);  // Update draftEmail state
      }

      console.log("Draft Email:", Emailresponse.data);
    } catch (err) {
      setDraftError("Failed to fetch draft email");
      console.error("Draft Email Error:", err);
    } finally {
      setDraftLoading(false);
    }
  };

  // Generate email with LLM
  const handleGenerateEmail = async () => {
    setLoading(true);
    setError(null);

    try {
      const LLMresponse = await axios.post("your-generate-email-api-endpoint", generatePayload, {
        headers: { "Content-Type": "application/json" },
      });

      setResponseData(LLMresponse.data);
      console.log("LLMresponse Email Response:", LLMresponse.data);
    } catch (err) {
      setError("Failed to generate email");
      console.error("Generate Email Error:", err);
    } finally {
      setLoading(false);
    }
  };

  return (
    <div className={styles.container}>
      {/* Left Section */}
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

      {/* Right Section */}
      <div className={styles.rightSection}>
        <div className={`${styles.sectionWindow} ${styles.draftEmail}`}>
          <h2>Draft Email</h2>
          {draftLoading ? (
            <p>Loading draft email...</p>
          ) : draftError ? (
            <p className={styles.errorText}>{draftError}</p>
          ) : (
            <p>{draftEmail}</p>  {/* Display email body here */}
          )}
          <button
            className={styles.fetchButton}
            onClick={handleFetchDraftEmail}
            disabled={draftLoading}
          >
            {draftLoading ? "Fetching Draft..." : "Generate Draft Email"}
          </button>
        </div>
        <div
          className={`${styles.sectionWindow} ${responseData ? styles.expanded : ""}`}
        >
          <h2>LLM Response</h2>
          <button
            className={styles.generateButton}
            onClick={handleGenerateEmail}
            disabled={loading}
          >
            {loading ? "LLMresponse Email..." : "LLMresponse Email"}
          </button>
          {error && <p className={styles.errorText}>{error}</p>}
          {responseData && (
            <p className={styles.successText}>
              Response: {JSON.stringify(responseData)}
            </p>
          )}
        </div>
        <button className={styles.submitButton}>Submit</button>
      </div>
    </div>
  );
};

export default GenerateEmail;
