import React from 'react';
import styles from "./ClaimProcessingStatus.module.css";

const ClaimProcessingStatus = ({ percentage, isLoading }) => {
  const emptyKeyPercentage = percentage ? parseFloat(percentage) : 0;
  const filledKeyPercentage = 100 - emptyKeyPercentage;

  return (
    <div>
      <h3 className={styles.percentHead}>Claim Processing Status</h3>

      {isLoading ? (
        <div className={styles.loadingSpinner}>Processing claim status...</div>
      ) : (
        <div className={styles.percentageContainer}>
          {/* Display Red and Green percentages side by side */}
          <div className={styles.percentageLabels}>
            <span className={styles.redText}>Red: {emptyKeyPercentage}%</span>
            <span className={styles.greenText}>Green: {filledKeyPercentage}%</span>
          </div>

          <div className={styles.splitPercentageBar}>
            <div
              className={styles.emptyKeysSection}
              style={{ width: `${emptyKeyPercentage}%` }}
            >
              {emptyKeyPercentage > 5 && `${emptyKeyPercentage}%`}
            </div>
            <div
              className={styles.filledKeysSection}
              style={{
                width: emptyKeyPercentage === 0 ? "100%" : `${filledKeyPercentage}%`,
              }}
            >
              {filledKeyPercentage > 5 && `${filledKeyPercentage}%`}
            </div>
          </div>
        </div>
      )}
    </div>
  );
};

export default ClaimProcessingStatus;




/* Display Red & Green percentage labels side by side */
.percentageLabels {
  display: flex;
  justify-content: space-between;
  font-size: 14px;
  font-weight: 600;
  margin-bottom: 5px;
}

.redText {
  color: #ff6b6b;
}

.greenText {
  color: #4ecb71;
}
