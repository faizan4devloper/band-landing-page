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
    claimId: "CL123456",
    recNumber: "PS391481",
    taskType: "FETCH_DRAFT_EMAIL",
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
      const response = await axios.post("your-draft-email-api-endpoint", draftPayload, {
        headers: { "Content-Type": "application/json" },
      });

      setDraftEmail(response.data.draft || "Draft email content not available.");
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
          <h2>Draft Email</h2>
          {draftLoading ? (
            <p>Loading draft email...</p>
          ) : draftError ? (
            <p className={styles.errorText}>{draftError}</p>
          ) : (
            <p>{draftEmail}</p>
          )}
          <button
            className={styles.fetchButton}
            onClick={handleFetchDraftEmail}
            disabled={draftLoading}
          >
            {draftLoading ? "Fetching Draft..." : "Fetch Draft Email"}
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
            {loading ? "Generating Email..." : "Generate Email"}
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




/* Draft Email Section */
.draftEmail {
  min-height: 250px; /* Increased height */
}

/* Fetch Draft Button */
.fetchButton {
  padding: 10px 15px;
  background-color: #ffa726;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1rem;
  margin-top: 10px;
  transition: background-color 0.2s ease;
}

.fetchButton:hover {
  background-color: #fb8c00;
}

.fetchButton:disabled {
  background-color: #ffcc80;
  cursor: not-allowed;
}

