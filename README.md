import React, { useState } from "react";
import axios from "axios";
import { useNavigate } from "react-router-dom";  // Correct import
import styles from "./MainContent.module.css";
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faSync } from '@fortawesome/free-solid-svg-icons';

const MainContent = ({ rows }) => {
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);
  const navigate = useNavigate();

  // Handle the "Verify" button click
  const handleVerify = async () => {
    setIsLoading(true);
    try {
      // Make the same API call used in handleReload
      const payload = {
        tasktype: "FETCH_SINGLE_ACT_CLAIM",
        claimid: rows[0]?.recNum,  // Using recNum from the first row as an example
      };

      const response = await axios.post("dummy-api-endpoint", payload, {
        headers: { "Content-Type": "application/json" },
      });

      // Assuming the response has summary and recommendations data
      const { summary, recommendations } = response.data;

      // Navigate to Verify page and pass data
      navigate("/verify", {
        state: { summary, recommendations },
      });
    } catch (error) {
      setError("Failed to fetch data.");
      console.error(error);
    } finally {
      setIsLoading(false);
    }
  };

  const handleReload = async (recNum) => {
    setIsLoading(true);
    try {
      const payload = {
        tasktype: "FETCH_SINGLE_ACT_CLAIM",
        claimid: recNum,
      };

      const response = await axios.post("dummy-api-endpoint", payload, {
        headers: { "Content-Type": "application/json" },
      });

      console.log("API Response:", response.data);
      // Handle the data accordingly (e.g., setting state)
    } catch (error) {
      console.error("Failed to fetch data for RecNum:", recNum, error);
    } finally {
      setIsLoading(false);
    }
  };

  return (
    <div className={styles.mainContent}>
      {/* Left and right windows as before */}
      <div className={styles.windowContainer}>
        <div className={styles.windowLeft}>
          <h3>Claim Summary</h3>
          <p>This is the claim summary content.</p>
        </div>

        <div className={styles.windowRight}>
          <h3>Recommendations</h3>
          <p>This is the recommendations content.</p>
        </div>
      </div>

      {/* Verify Button */}
      <div className={styles.buttonContainer}>
        <button
          className={styles.verifyButton}
          onClick={handleVerify}
          disabled={isLoading}
        >
          {isLoading ? "Verifying..." : "Verify"}
        </button>
      </div>

      {/* Reload Button */}
      {rows.map((row, index) => (
        <div key={index}>
          <button
            className={styles.reloadButton}
            onClick={() => handleReload(row.recNum)} // Use dynamic recNum here
            disabled={isLoading}
          >
            <FontAwesomeIcon
              icon={faSync}
              spin={isLoading} // Spin when loading is true
              className={styles.icon}
            />
            {!isLoading && " Reload"} {/* Show text only when not loading */}
          </button>
        </div>
      ))}
    </div>
  );
};

export default MainContent;



import React from "react";
import { useLocation } from "react-router-dom";
import styles from "./Verify.module.css";  // Ensure this file exists for styling

const Verify = () => {
  // Get passed data (summary and recommendations) using useLocation hook
  const location = useLocation();
  const { summary, recommendations } = location.state || {};

  return (
    <div className={styles.verifyPage}>
      <div className={styles.windowContainer}>
        {/* Left side: Claim Summary */}
        <div className={styles.windowLeft}>
          <h3>Claim Summary</h3>
          {summary ? (
            <div>
              <p>{summary}</p>
            </div>
          ) : (
            <p>No summary available.</p>
          )}
        </div>

        {/* Right side: Recommendations */}
        <div className={styles.windowRight}>
          <h3>Recommendations</h3>
          {recommendations ? (
            <div>
              <p>{recommendations}</p>
            </div>
          ) : (
            <p>No recommendations available.</p>
          )}
        </div>
      </div>
    </div>
  );
};

export default Verify;
