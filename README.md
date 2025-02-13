import React from 'react';
import styles from "./ClaimProcessingStatus.module.css";

const ClaimProcessingStatus = ({ percentage, isLoading }) => {
  const progress = percentage !== undefined 
    ? Math.max(0, Math.min(100, parseFloat(percentage))) 
    : 0;

  return (
    <div className={styles.container}>
      <h3 className={styles.percentHead}>Claim Processing Status</h3>

      {isLoading ? (
        <div className={styles.loadingContainer}>
          <div className={styles.loadingSpinner}></div>
          <p className={styles.loadingText}>Processing claim status...</p>
        </div>
      ) : (
        <div className={styles.percentageContainer}>
          <div className={styles.progressBar}>
            <div
              className={styles.progressFill}
              style={{ width: `${progress}%` }}
            >
              {progress > 10 && <span className={styles.progressText}>{progress.toFixed()}%</span>}
            </div>
          </div>
          <p className={styles.statusLabel}>
            {progress < 100 ? "In Progress..." : "Completed"}
          </p>
        </div>
      )}
    </div>
  );
};

export default ClaimProcessingStatus;


/* Container */
.container {
  text-align: center;
  padding: 15px;
  background: #fff;
  border-radius: 12px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  max-width: 400px;
  margin: auto;
}

/* Heading */
.percentHead {
  color: #2c3e50;
  font-size: 1.4rem;
  font-weight: bold;
  margin-bottom: 15px;
}

/* Loading State */
.loadingContainer {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
}

.loadingText {
  font-size: 14px;
  color: #555;
}

.loadingSpinner {
  width: 30px;
  height: 30px;
  border: 3px solid rgba(0, 0, 0, 0.1);
  border-top: 3px solid #3498db;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

/* Progress Bar */
.percentageContainer {
  margin-top: 15px;
}

.progressBar {
  width: 100%;
  height: 30px;
  background-color: #ecf0f1;
  border-radius: 18px;
  overflow: hidden;
  position: relative;
  box-shadow: inset 0 1px 4px rgba(0, 0, 0, 0.2);
}

.progressFill {
  height: 100%;
  background: linear-gradient(90deg, #4ecb71, #2ecc71);
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
  font-size: 14px;
  color: white;
  transition: width 0.6s ease-in-out;
}

.progressText {
  font-size: 14px;
  font-weight: bold;
}

.statusLabel {
  margin-top: 10px;
  font-size: 14px;
  font-weight: 600;
  color: #444;
}
