import React, { useState } from "react";
import axios from "axios";
import styles from "./GenerateEmail.module.css";

const GenerateEmail = () => {
  const [loading, setLoading] = useState(false);
  const [draftLoading, setDraftLoading] = useState(false);
  const [error, setError] = useState(null);
  const [draftError, setDraftError] = useState(null);
  const [responseData, setResponseData] = useState(null);
  const [draftEmail, setDraftEmail] = useState(""); // Store draft email content as a string

  // API payloads
  const draftPayload = {
    claimid: "CL1234567",
    recnumber: "PS391481",
    tasktype: "FETCH_EMAIL",
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

      // Extract emailbody and update state
      if (Emailresponse.data && typeof Emailresponse.data.emailbody === "string") {
        setDraftEmail(Emailresponse.data.emailbody);
      } else {
        setDraftEmail("No valid draft email found.");
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
      console.log("LLM Response:", LLMresponse.data);
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
        {/* Draft Email Section */}
        <div className={`${styles.sectionWindow} ${styles.draftEmail}`}>
          <h2>Draft Email</h2>
          {draftLoading ? (
            <p>Loading draft email...</p>
          ) : draftError ? (
            <p className={styles.errorText}>{draftError}</p>
          ) : draftEmail ? (
            <p>{draftEmail}</p>
          ) : (
            <p>No draft email available.</p>
          )}
          <button
            className={styles.fetchButton}
            onClick={handleFetchDraftEmail}
            disabled={draftLoading}
          >
            {draftLoading ? "Fetching Draft..." : "Generate Draft Email"}
          </button>
        </div>

        {/* LLM Response Section */}
        <div
          className={`${styles.sectionWindow} ${responseData ? styles.expanded : ""}`}
        >
          <h2>LLM Response</h2>
          <button
            className={styles.generateButton}
            onClick={handleGenerateEmail}
            disabled={loading}
          >
            {loading ? "Generating Email..." : "Generate Email"}
          </button>
          {error && <p className={styles.errorText}>{error}</p>}
          {responseData && (
            <pre className={styles.successText}>
              Response: {JSON.stringify(responseData, null, 2)}
            </pre>
          )}
        </div>

        <button className={styles.submitButton}>Submit</button>
      </div>
    </div>
  );
};

export default GenerateEmail;
