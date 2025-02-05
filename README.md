import React from "react";
import styles from "./ClaimProcessingStatus.module.css";

const ClaimProcessingStatus = ({ percentage, isLoading }) => {
  // Ensure percentage is valid and within 0-100
  const progress = percentage !== undefined 
    ? Math.max(0, Math.min(100, parseFloat(percentage))) 
    : 0;

  return (
    <div className={styles.statusWrapper}>
      <h3 className={styles.percentHead}>Claim Processing Status</h3>

      {isLoading ? (
        <div className={styles.loadingContainer}>
          <div className={styles.loadingBar}></div>
          <span className={styles.loadingText}>Processing claim status...</span>
        </div>
      ) : (
        <div className={styles.progressBarContainer}>
          <div className={styles.progressBar} style={{ width: `${progress}%` }}>
            <span className={styles.progressText}>{progress.toFixed()}%</span>
          </div>
        </div>
      )}
    </div>
  );
};

export default ClaimProcessingStatus;




.statusWrapper {
  width: 100%;
  max-width: 400px;
  margin: 20px auto;
  text-align: center;
}

.percentHead {
  color: #2c3e50;
  font-size: 1.4rem;
  font-weight: 600;
  margin-bottom: 10px;
}

/* Loading state */
.loadingContainer {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 6px;
}

.loadingBar {
  width: 100%;
  height: 6px;
  border-radius: 3px;
  background: linear-gradient(90deg, #ddd 25%, #bbb 50%, #ddd 75%);
  background-size: 200% 100%;
  animation: shimmer 1.5s infinite linear;
}

.loadingText {
  font-size: 14px;
  color: #666;
}

/* Progress Bar */
.progressBarContainer {
  width: 100%;
  height: 30px;
  background-color: #f0f0f0;
  border-radius: 18px;
  overflow: hidden;
  position: relative;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
}

.progressBar {
  height: 100%;
  background: linear-gradient(90deg, #4ecb71, #3ba55d);
  transition: width 0.6s ease-in-out;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #ffffff;
  font-weight: 600;
  font-size: 14px;
  border-radius: 18px;
}

.progressText {
  white-space: nowrap;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.2);
}

/* Loading shimmer effect */
@keyframes shimmer {
  0% { background-position: -200% 0; }
  100% { background-position: 200% 0; }
}
