import React, { useState } from "react";
import axios from "axios";
import "./GenerateEmail.css"; // Add styles for layout

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
    <div className="generate-email-container">
      <div className="left-section">
        <div className="section-window claim-summary">
          <h2>Claim Summary</h2>
          <p>Details about the claim go here.</p>
        </div>
        <div className="section-window recommendations">
          <h2>Recommendations</h2>
          <p>Suggestions based on the claim details go here.</p>
        </div>
      </div>
      <div className="right-section">
        <div className="section-window draft-email">
          <h2>Draft Email</h2>
          <button onClick={handleGenerateEmail} disabled={loading}>
            {loading ? "Generating Email..." : "Generate Email"}
          </button>
          {error && <p style={{ color: "red" }}>{error}</p>}
          {responseData && <p>Email Generated Successfully!</p>}
        </div>
        <div className="section-window llm-response">
          <h2>LLM Response</h2>
          <p>Generated response from the LLM goes here.</p>
        </div>
        <button className="submit-button">Submit</button>
      </div>
    </div>
  );
};

export default GenerateEmail;



.generate-email-container {
  display: flex;
  gap: 20px;
  padding: 20px;
  background-color: #f9f9f9;
  min-height: 100vh;
  font-family: Arial, sans-serif;
}

.left-section,
.right-section {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.section-window {
  background: #ffffff;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  padding: 20px;
}

.section-window h2 {
  margin-bottom: 10px;
  font-size: 1.25rem;
}

.section-window p {
  color: #555;
}

.submit-button {
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

.submit-button:hover {
  background-color: #45a049;
}
