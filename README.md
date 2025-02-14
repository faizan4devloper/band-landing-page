import React from "react";
import styles from "./ClaimProcessingStatus.module.css";

const ClaimProcessingStatus = ({ percentage }) => {
  // Check if percentage is available
  const isLoading = percentage === undefined;

  // Ensure percentage is valid, default to 0
  const emptyKeyPercentage = !isLoading
    ? Math.max(0, Math.min(100, parseFloat(percentage)))
    : 0;

  const filledKeyPercentage = 100 - emptyKeyPercentage;

  return (
    <div>
      <h3 className={styles.percentHead}>Claim Processing Status</h3>

      <div className={styles.percentageContainer}>
        {isLoading ? (
          <div className={styles.loadingBar}>
            <div className={styles.loadingAnimation}></div>
          </div>
        ) : (
          <div className={styles.splitPercentageBar}>
            <div
              className={styles.emptyKeysSection}
              style={{ width: `${emptyKeyPercentage}%` }}
            >
              {emptyKeyPercentage > 0 && `${emptyKeyPercentage.toFixed()}%`}
            </div>

            <div
              className={styles.filledKeysSection}
              style={{
                width: emptyKeyPercentage === 0 ? "100%" : `${filledKeyPercentage}%`,
              }}
            >
              {filledKeyPercentage > 0 && `${filledKeyPercentage.toFixed()}%`}
            </div>
          </div>
        )}
      </div>
    </div>
  );
};

export default ClaimProcessingStatus;



.percentHead {
  color: #2c3e50;
  font-size: 1.2rem;
  font-weight: 600;
  margin: 0;
  display: flex;
  align-items: center;
}

.percentageContainer {
  width: 100%;
  height: 30px;
  background-color: #f0f0f0;
  border-radius: 18px;
  overflow: hidden;
  margin-top: 12px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
  position: relative;
}

.splitPercentageBar {
  display: flex;
  width: 100%;
  height: 100%;
}

.emptyKeysSection,
.filledKeysSection {
  display: flex;
  align-items: center;
  justify-content: center;
  color: #ffffff;
  font-weight: 600;
  font-size: 14px;
  min-width: 12%;
  transition: width 0.6s ease-in-out;
  padding: 0 8px;
  white-space: nowrap;
}

.emptyKeysSection {
  background-color: #ff6b6b;
}

.filledKeysSection {
  background-color: #4ecb71;
}

/* Loader effect inside progress bar */
.loadingBar {
  width: 100%;
  height: 100%;
  background: linear-gradient(45deg, #e0e0e0, #c0c0c0);
  position: relative;
  overflow: hidden;
}

.loadingAnimation {
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, rgba(255, 255, 255, 0.1) 25%, rgba(255, 255, 255, 0.3) 50%, rgba(255, 255, 255, 0.1) 75%);
  background-size: 200% 100%;
  animation: loadingAnimation 1.5s infinite linear;
}

@keyframes loadingAnimation {
  0% { background-position: -200% 0; }
  100% { background-position: 200% 0; }
}
