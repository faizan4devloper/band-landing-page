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
          {/* Display Red percentage only if it's not 100% green */}
          {emptyKeyPercentage > 0 && (
            <div className={styles.redText}>Red: {emptyKeyPercentage}%</div>
          )}

          <div className={styles.splitPercentageBar}>
            {emptyKeyPercentage > 0 && (
              <div
                className={styles.emptyKeysSection}
                style={{ width: `${emptyKeyPercentage}%` }}
              >
                {emptyKeyPercentage > 5 && `${emptyKeyPercentage}%`}
              </div>
            )}
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

      <p className={styles.statusLabel}>
        {emptyKeyPercentage > 50 ? "Needs Attention" : "Looking Good!"}
      </p>
    </div>
  );
};

export default ClaimProcessingStatus;



.redText {
  color: #ff6b6b;
  font-size: 14px;
  font-weight: 600;
  text-align: left;
  margin-bottom: 5px;
}
