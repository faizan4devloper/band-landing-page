
import React, { useState } from "react";
import axios from "axios";
import styles from "./GenerateEmail.module.css";

const GenerateEmail = () => {
  const [loading, setLoading] = useState(false);
  const [draftLoading, setDraftLoading] = useState(false);
  const [error, setError] = useState(null);
  const [draftError, setDraftError] = useState(null);
  const [responseData, setResponseData] = useState(null);
  const [draftEmail, setDraftEmail] = useState("");

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
      const response = await axios.post("dummy", draftPayload, {
        headers: { "Content-Type": "application/json" },
      });

      console.log("Draft API:", response.data);

      setDraftEmail(response.data.emailbody || "Draft email content not available.");
      console.log("Draft Email:", response.data);
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
      const response = await axios.post("your-generate-email-api-endpoint", generatePayload, {
        headers: { "Content-Type": "application/json" },
      });

      setResponseData(response.data);
      console.log("Generated Email Response:", response.data);
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
          <button
            className={styles.fetchButton}
            onClick={handleFetchDraftEmail}
            disabled={draftLoading}
          >
            {draftLoading ? "Fetching Draft..." : "Fetch Draft Email"}
          </button>
          {/* Display the fetched draft email content */}
          {draftLoading ? (
            <p>Loading draft email...</p>
          ) : draftError ? (
            <p className={styles.error}>{draftError}</p>
          ) : (
            draftEmail && (
              <div className={styles.emailContent}>
                <h3>Draft Email:</h3>
                <pre>{draftEmail}</pre>
              </div>
            )
          )}
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
            {loading ? "LLM Email..." : "LLM Email"}
          </button>
        </div>
        <button className={styles.submitButton}>Submit</button>
      </div>
    </div>
  );
};

export default GenerateEmail;
