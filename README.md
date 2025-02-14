import React from "react";
import PropTypes from "prop-types";
import styles from "./ClaimSummary.module.css";

const ClaimSummary = ({ Dsummary, recommendation, summary }) => {
  return (
    <div className={styles.claimSummaryContainer}>
      {/* Claim Summary Section */}
      <div className={styles.summarySection}>
        <h3>Claim Summary</h3>
        <p>{summary || "No summary available"}</p>
      </div>
      
      {/* Detailed Summary Section */}
      <div className={styles.detailSummarySection}>
        <h3>Detailed Summary</h3>
        <p>{Dsummary || "No detailed summary available"}</p>
      </div>
      
      {/* Recommendation Section */}
      <div className={styles.recommendationSection}>
        <h3>Recommendation</h3>
        <p>{recommendation || "No recommendation available"}</p>
      </div>
    </div>
  );
};

ClaimSummary.propTypes = {
  Dsummary: PropTypes.string,
  recommendation: PropTypes.string,
  summary: PropTypes.string,
};

export default ClaimSummary;
