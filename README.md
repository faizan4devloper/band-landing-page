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
        const response = await axios.post("dummy", payload, {
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
      claimid: "CL1234567",
      recnumber: "PS391481",
      tasktype: "FETCH_EMAIL",
    };

    setLoadingLLM(true);
    setErrorLLM(null);

    try {
      const response = await axios.post("dummy", payload, {
        headers: { "Content-Type": "application/json" },
      });

      // Safely parse response
      const llmResponseString = response.data?.emailbody?.emailbody;
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
              <pre className={styles.emailContent}>{parsedEmailBody}</pre>
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
              <pre className={styles.emailContent}>{llmResponse}</pre>
            </div>
          )}
        </div>
        <button className={styles.submitButton}>Submit</button>
      </div>
    </div>
  );
};

export default GenerateEmail;
