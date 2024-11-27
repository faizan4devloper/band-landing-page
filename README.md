import React, { useState } from "react";
import axios from "axios";
import styles from "./GenerateEmail.module.css"; // Import CSS Module

const GenerateEmail = () => {
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [responseData, setResponseData] = useState(null);

  const handleGenerateEmail = async () => {
    const payload = {
      claimid: "CL123456",
      recnumber: "PS391481",
      tasktype: "GENERATE_EMAIL"
    };

    setLoading(true);
    setError(null);

    try {
      const response = await axios.post("dummy", payload, {
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
          {responseData && <p className={styles.successText}>Email Generated Successfully!</p>}
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







.container {
  display: flex;
  gap: 20px;
  padding: 20px;
  background-color: #f9f9f9;
  min-height: 100vh;
  font-family: Arial, sans-serif;
}

.leftSection,
.rightSection {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.sectionWindow {
  background: #ffffff;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  padding: 20px;
}

.sectionWindow h2 {
  margin-bottom: 10px;
  font-size: 1.25rem;
}

.sectionWindow p {
  color: #555;
}

.generateButton {
  padding: 10px 15px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1rem;
}

.generateButton:disabled {
  background-color: #b0c4de;
  cursor: not-allowed;
}

.submitButton {
  align-self: flex-end;
  padding: 10px 20px;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1rem;
  margin-top: 20px;
}

.submitButton:hover {
  background-color: #45a049;
}

.errorText {
  color: red;
}

.successText {
  color: green;
}
