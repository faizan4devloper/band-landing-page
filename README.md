import React, { useState } from "react";
import styles from "./MainContent.module.css";

const MainContent = () => {
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

export default MainContent;




/* MainContent.module.css */
.mainContent {
  padding: 20px;
  font-family: Arial, sans-serif;
  background-color: #f4f7fc; /* Background color for the whole section */
}

.windowContainer {
  display: flex;
  justify-content: space-between;
  gap: 20px;
  margin-bottom: 20px;
}

.windowLeft,
.windowRight {
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  padding: 20px;
  width: 48%;
}

.windowLeft h3,
.windowRight h3 {
  color: #333;
  font-size: 24px;
  margin-bottom: 10px;
}

.windowLeft p,
.windowRight p {
  color: #555;
  font-size: 16px;
  line-height: 1.6;
}

.buttonContainer {
  display: flex;
  justify-content: center;
}

.generateButton {
  background-color: #4CAF50;
  color: white;
  padding: 15px 25px;
  border: none;
  border-radius: 5px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.generateButton:hover {
  background-color: #45a049;
}

.generateButton:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}
