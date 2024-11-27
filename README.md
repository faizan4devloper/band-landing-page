import React, { useState } from "react";
import styles from "./Verify.module.css";

const Verify = () => {
  const [isLoading, setIsLoading] = useState(false);

  // Handle the "Generate Email" button click
  const handleGenerateEmail = () => {
    setIsLoading(true);
    // Logic to generate email goes here
    setTimeout(() => {
      alert("Email Generated Successfully!");
      setIsLoading(false);
    }, 2000);
  };

  return (
    <div className={styles.mainContent}>
      {/* Flex container for two beautiful windows */}
      <div className={styles.windowContainer}>
        {/* Left window for Claim Summary */}
        <div className={styles.windowLeft}>
          <h3>Claim Summary</h3>
          <p>This is the claim summary content.</p>
          {/* You can add dynamic content here */}
        </div>

        {/* Right window for Recommendations */}
        <div className={styles.windowRight}>
          <h3>Recommendations</h3>
          <p>This is the recommendations content.</p>
          {/* You can add dynamic content here */}
        </div>
      </div>

      {/* Generate Email Button */}
      <div className={styles.buttonContainer}>
        <button
          className={styles.generateButton}
          onClick={handleGenerateEmail}
          disabled={isLoading}
        >
          {isLoading ? "Generating..." : "Generate Email"}
        </button>
      </div>
    </div>
  );
};

export default Verify;


