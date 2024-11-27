import React, { useState } from "react";
import axios from "axios";
import { useNavigate } from "react-router-dom";
import styles from "./MainContent.module.css";
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faSync } from '@fortawesome/free-solid-svg-icons';

const MainContent = ({ rows }) => {
  const [loading, setLoading] = useState(false);
  const [data, setData] = useState(null); // To store fetched data
  const navigate = useNavigate();

  const handleReload = async (recNum) => {
    setLoading(true);
    try {
      const payload = {
        tasktype: "FETCH_SINGLE_ACT_CLAIM",
        claimid: recNum,
      };
      const response = await axios.post(`dummy`, payload, {
        headers: { "Content-Type": "application/json" },
      });
      
      console.log("Api", response.data);
      setData(response.data.allclaimactdata); // Store fetched data
    } catch (error) {
      console.error("Failed to fetch data for RecNum:", recNum, error);
    } finally {
      setLoading(false);
    }
  };

  const handleVerify = () => {
    const claimSummary = data?.CLAIM_FORM_DETAILS?.CLAIM_FORM_DETAILED_SUMMARY;

    // Pass the claim summary to Verify page through state
    navigate("/verify", {
      state: {
        claimSummary, // Pass the detailed summary here
      },
    });
  };

  const renderReadableContent = (data) => {
    if (!data) return <p>No data available</p>;

    const parsedData = JSON.parse(data);
    return (
      <div>
        <h4>Claim Form Details</h4>
        <p><strong>Type:</strong> {parsedData.CLAIM_FORM_DETAILS?.CLAIM_FORM_TYPE}</p>
        <p><strong>Claim Detailed Summary:</strong> {parsedData.CLAIM_FORM_DETAILS?.CLAIM_FORM_DETAILED_SUMMARY}</p>
      </div>
    );
  };

  return (
    <div className={styles.mainContent}>
      <div className={styles.extractContentSection}>
        <h3>Extracted Content</h3>
        {loading ? (
          <p>Loading...</p>
        ) : (
          renderReadableContent(data?.total_extracted_data)
        )}
        
        {rows.map((row, index) => (
          <div key={index}>
            <button
              className={styles.reloadButton}
              onClick={() => handleReload(row.recNum)} // Use dynamic recNum here
              disabled={loading}
            >
              <FontAwesomeIcon
                icon={faSync}
                spin={loading}
                className={styles.icon}
              />
              {!loading && " Reload"}
            </button>
          </div>
        ))}
      </div>

      <div className={styles.verifyButtonContainer}>
        <button className={styles.verifyButton} onClick={handleVerify}>
          Verify
        </button>
      </div>
    </div>
  );
};

export default MainContent;





import React, { useEffect, useState } from "react";
import { useLocation } from "react-router-dom";
import { HashLoader } from "react-spinners";
import styles from "./Verify.module.css";

const Verify = () => {
  const location = useLocation();

  const [summary, setSummary] = useState(null);
  const [recommendation, setRecommendation] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    if (location.state) {
      // Directly receive data from state
      const { claimSummary } = location.state;
      setSummary(claimSummary); // Setting claim summary as summary
      setRecommendation(claimSummary); // You can also customize this as per your data
      setLoading(false);
    } else {
      setError("No data available.");
      setLoading(false);
    }
  }, [location.state]);

  if (loading) {
    return (
      <div className={styles.spinnerContainer}>
        <HashLoader color="#0f5fdc" size={40} />
      </div>
    );
  }

  if (error) {
    return <p>{error}</p>;
  }

  return (
    <div className={styles.verifyContainer}>
      {/* Left side: Claim Summary */}
      <div className={styles.leftPanel}>
        <h3>Claim Summary</h3>
        <p><strong>Claim Detailed Summary:</strong> {summary}</p>
      </div>

      {/* Right side: Recommendations */}
      <div className={styles.rightPanel}>
        <h3>Detailed Summary</h3>
        <p>{recommendation}</p>
      </div>

      <div className={styles.generateEmailContainer}>
        <button className={styles.generateEmailButton}>Generate Email</button>
      </div>
    </div>
  );
};

export default Verify;
