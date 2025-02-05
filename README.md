import React from 'react';
import styles from "./ClaimProcessingStatus.module.css";

const ClaimProcessingStatus = ({ percentage, isLoading }) => {
  // Ensure percentage is a number, default to 0 if not
  const processedPercentage = percentage !== undefined 
    ? Math.max(0, Math.min(100, parseFloat(percentage)))
    : 0;

  return (
    <div className={styles.container}>
      <h3 className={styles.percentHead}>Claim Processing Status</h3>

      {isLoading ? (
        <div className={styles.loadingContainer}>
          <div className={styles.spinner}></div>
          <p className={styles.loadingText}>Processing claim status...</p>
        </div>
      ) : (
        <div className={styles.percentageContainer}>
          <div className={styles.progressBar}>
            <div
              className={styles.filledSection}
              style={{ width: `${processedPercentage}%` }}
            >
              {processedPercentage > 0 && `${processedPercentage.toFixed()}%`}
            </div>
          </div>
        </div>
      )}
    </div>
  );
};

export default ClaimProcessingStatus;


.container {
  width: 100%;
  max-width: 400px;
  margin: auto;
}

.percentHead {
  color: #2c3e50;
  font-size: 1.2rem;
  font-weight: 600;
  margin-bottom: 12px;
  text-align: center;
}

.percentageContainer {
  width: 100%;
  height: 30px;
  background-color: #f0f0f0;
  border-radius: 18px;
  overflow: hidden;
  position: relative;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
}

.progressBar {
  display: flex;
  width: 100%;
  height: 100%;
}

.filledSection {
  display: flex;
  align-items: center;
  justify-content: center;
  color: #ffffff;
  font-weight: 600;
  font-size: 14px;
  transition: width 0.8s ease-in-out;
  white-space: nowrap;
  background: linear-gradient(90deg, #4ecb71, #2ecc71);
}

.loadingContainer {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.spinner {
  width: 24px;
  height: 24px;
  border: 3px solid rgba(0, 0, 0, 0.1);
  border-top: 3px solid #4ecb71;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

.loadingText {
  font-size: 12px;
  color: #666;
  margin-top: 8px;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
