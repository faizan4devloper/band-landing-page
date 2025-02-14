
import React from 'react';
import styles from "./ClaimProcessingStatus.module.css";

const ClaimProcessingStatus = ({ percentage, isLoading }) => {
  // Ensure percentage is a number, default to 0 if not
  const emptyKeyPercentage = percentage !== undefined 
    ? Math.max(0, Math.min(100, parseFloat(percentage)))
    : 0;
  
  const filledKeyPercentage = 100 - emptyKeyPercentage;

  return (
    <div>
      <h3 className={styles.percentHead}>Claim Processing Status</h3>

      {isLoading ? (
        <div className={styles.loadingSpinner}>Processing claim status...</div>
      ) : (
        <div className={styles.percentageContainer}>
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
        </div>
      )}
    </div>
  );
};

export default ClaimProcessingStatus;

.percentHead{
      color: #2c3e50;
    font-size: 1.2rem;
    font-weight: 600;
    margin: 0;
    display: flex
;
    align-items: center;
}

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

/*.redText {*/
/*  color: #ff6b6b;*/
/*  font-size: 14px;*/
/*  font-weight: 600;*/
/*  text-align: left;*/
/*  margin-bottom: 5px;*/
/*}*/

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
  min-width: 12%; /* Prevents text from getting cut off */
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

.statusLabel {
  font-size: 14px;
  margin-top: 8px;
  text-align: center;
  color: #444;
  font-weight: 500;
}
.loadingSpinner{
  text-align: center;
  font-size: 12px;
}

