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
  // const recNum = location.state?.recNum || null;
    const recNum = location.state?.recNum || "CL1234567"

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
        claimid: recNum,
        psid: "PS391481",
        tasktype: "FETCH_EMAIL",
      };

      setLoadingEmail(true);
      setErrorEmail(null);

      try {
        const response = await axios.post("https://e21wxu9skj.execute-api.us-east-1.amazonaws.com/dev/querequest", payload, {
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
      claimid: recNum,
      psid: "PS391481",
      tasktype: "GENERATE_EMAIL",
    };

    setLoadingLLM(true);
    setErrorLLM(null);

    try {
      const response = await axios.post("https://e21wxu9skj.execute-api.us-east-1.amazonaws.com/dev/querequest", payload, {
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
    <div>
      
       <div className={styles.claimIdDisplay}>
          <h3>Claim ID: {recNum}</h3>  
          </div>
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
          <h2>Template Based Email</h2>
          {loadingEmail && <p>Loading email content...</p>}
          {errorEmail && <p className={styles.errorText}>{errorEmail}</p>}
          {parsedEmailBody && (
            <div className={styles.emailPreview}>
              <p className={styles.emailContent}>{parsedEmailBody}</p>
            </div>
          )}
        </div>
        <div className={styles.sectionWindow}>
          <h2>Generated Email</h2>
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
        </div>

  );
};

export default GenerateEmail;




/* Main Container Styling */
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
  padding: 15px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border: 1px solid #d0d0d0;
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
  font-size: .9rem;
  line-height: 1.5;
  
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
    'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue',
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* Draft Email Section */
.draftEmail {
  min-height: 250px; /* Increased height */
}

/* Expanded Section */
.expanded {
  min-height: 200px;
  background-color: #f3f3f3;
  transition: height 0.3s ease;
}

/* Email Content Styling */
.emailContent {
  white-space: pre-wrap; /* Preserve line breaks and spacing */
  background-color: #f8f9fa;
  border: 1px solid #d0d0d0;
  border-radius: 8px;
  padding: 15px;
  margin-top: 15px;
  color: #333;
  font-family: 'Courier New', Courier, monospace;
  font-size: 0.95rem;
  line-height: 1.6;
  overflow-y: auto; /* Add scroll for long content */
  max-height: 400px; /* Limit height for large content */
  box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.1);
}

.emailContent::-webkit-scrollbar {
  width: 8px;
}

.emailContent::-webkit-scrollbar-thumb {
  background: #b0c4de;
  border-radius: 4px;
}

.emailContent::-webkit-scrollbar-thumb:hover {
  background: #7fa2cd;
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
.claimIdDisplay{
     margin: 0px 25px;
    font-size: 16px;
    margin-bottom: 0px;
    font-weight: bold;
    color: #333;
}
