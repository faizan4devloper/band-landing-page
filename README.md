import React, { useState } from "react";
import axios from "axios";
import styles from "./GenerateEmail.module.css";

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
          <p>Enter your draft email content or view suggestions.</p>
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







.container {
  display: flex;
  gap: 20px;
  padding: 20px;
  background-color: #f9f9f9;
  min-height: 100vh;
  font-family: Arial, sans-serif;
}

/* Left and Right Section Styling */
.leftSection,
.rightSection {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 20px;
  background-color: #f0f0f0;
  padding: 15px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border: 1px solid #d0d0d0;
}

.leftSection {
  background-color: #e3f2fd;
}

.rightSection {
  background-color: #fff8e1;
}

/* Individual Window Styling */
.sectionWindow {
  background: #ffffff;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  padding: 20px;
  transition: transform 0.2s ease, box-shadow 0.2s ease, height 0.3s ease;
}

.sectionWindow:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 12px rgba(0, 0, 0, 0.2);
}

.sectionWindow h2 {
  margin-bottom: 10px;
  font-size: 1.4rem;
  color: #333;
}

.sectionWindow p {
  color: #555;
  font-size: 1rem;
  line-height: 1.5;
}

/* Draft Email Section */
.draftEmail {
  min-height: 200px; /* Increased height */
}

/* Expanded Section */
.expanded {
  min-height: 200px;
  background-color: #f3f3f3;
  transition: height 0.3s ease;
}

/* Buttons */
.generateButton {
  padding: 10px 15px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1rem;
  transition: background-color 0.2s ease;
}

.generateButton:hover {
  background-color: #0056b3;
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
  transition: background-color 0.2s ease;
}

.submitButton:hover {
  background-color: #45a049;
}

/* Error and Success Messages */
.errorText {
  color: red;
  font-weight: bold;
}

.successText {
  color: green;
  font-weight: bold;
}
