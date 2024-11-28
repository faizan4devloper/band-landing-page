import React, { useState, useEffect } from "react";
import axios from "axios";
import styles from "./GenerateEmail.module.css"; // Import CSS Module

const GenerateEmail = () => {
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [parsedEmailBody, setParsedEmailBody] = useState("");

  useEffect(() => {
    const fetchEmail = async () => {
      const payload = {
        claimid: "CL1234567",
        recnumber: "PS391481",
        tasktype: "FETCH_EMAIL",
      };

      setLoading(true);
      setError(null);

      try {
        const response = await axios.post("dummy", payload, {
          headers: { "Content-Type": "application/json" },
        });

        // Safely parse response
        const emailBodyString = response.data?.emailbody?.emailbody;
        if (emailBodyString) {
          const parsedBody = emailBodyString.trim();
          setParsedEmailBody(parsedBody);
        } else {
          throw new Error("Invalid email body format");
        }
      } catch (err) {
        setError("Failed to fetch email");
        console.error("Error:", err);
      } finally {
        setLoading(false);
      }
    };

    fetchEmail();
  }, []); // Runs only once on component mount

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
          {loading && <p>Loading email content...</p>}
          {error && <p className={styles.errorText}>{error}</p>}
          {parsedEmailBody && (
            <div className={styles.emailPreview}>
              <h3>Email Body:</h3>
              <pre className={styles.emailContent}>{parsedEmailBody}</pre>
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
