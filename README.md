const handleVerify = () => {
  const extractedData = JSON.parse(data?.total_extracted_data || "{}");
  const detailedSummary = extractedData.CLAIM_FORM_DETAILS?.CLAIM_FORM_DETAILED_SUMMARY;

  navigate("/verify", { state: { detailedSummary } });
};



import React from "react";
import { useLocation } from "react-router-dom";
import styles from "./Verify.module.css";

const Verify = () => {
  const location = useLocation();
  const { detailedSummary } = location.state || {};

  return (
    <div className={styles.verifyContainer}>
      <h3>Claim Form Detailed Summary</h3>
      {detailedSummary ? (
        <p>{detailedSummary}</p>
      ) : (
        <p>No detailed summary available.</p>
      )}
    </div>
  );
};

export default Verify;
