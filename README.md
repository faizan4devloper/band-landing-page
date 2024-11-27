import React, { useState } from "react";
import axios from "axios";
import styles from "./GenerateEmail.module.css"; // Import CSS Module

const GenerateEmail = () => {
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [responseData, setResponseData] = useState(null);

  const handleGenerateEmail = async () => {
    const payload = {
      claimid: "CL1234567",
      recnumber: "PS391481",
      tasktype: "FETCH_EMAIL"
    };

    setLoading(true);
    setError(null);

    try {
      const response = await axios.post("DUMMY", payload, {
        headers: { "Content-Type": "application/json" },
      });

      setResponseData(response.data);
      console.log("Response Data:", response.data);
    } catch (err) {
      setError("Failed to generate email");
      console.error("Error:", err);
    } finally {
      setLoading(false);
    }
  };

  // Helper function to format email body
  const formatEmailBody = (emailBody) => {
    if (!emailBody) return "";
    return emailBody
      .split("\n") // Split text into lines
      .map((line, index) => (
        <p key={index} style={{ marginBottom: "10px" }}>
          {line.trim()}
        </p>
      )); // Trim and wrap each line in a paragraph
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
          <button
            className={styles.generateButton}
            onClick={handleGenerateEmail}
            disabled={loading}
          >
            {loading ? "Generating Email..." : "Generate Email"}
          </button>
          {error && <p className={styles.errorText}>{error}</p>}
          {responseData && (
            <div className={styles.emailBody}>
              <h3>Email Content</h3>
              {formatEmailBody(responseData.emailbody)}
            </div>
          )}
        </div>
        <div className={styles.sectionWindow}>
          <h2>LLM Response</h2>
          <p>Generated response from the LLM goes here.</p>
        </div>
        <button className={styles.submitButton}>Submit</button>
      </div>
    </div>
  );
};

export default GenerateEmail;
