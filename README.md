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
          {/* Display red & green percentages above the bar */}
          <div className={styles.percentageLabel}>
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

      <p className={styles.statusLabel}>
        {emptyKeyPercentage > 50 ? "Needs Attention" : "Looking Good!"}
      </p>
    </div>
  );
};

export default ClaimProcessingStatus;



/* Label styles above the percentage bar */
.percentageLabel {
  display: flex;
  justify-content: space-between;
  margin-bottom: 5px;
  font-size: 14px;
  font-weight: 600;
}

.redText {
  color: #ff6b6b;
}

.greenText {
  color: #4ecb71;
}







i want also display red ones percentage ex:- like red 3% green 97%

import React from 'react';
import styles from "./ClaimProcessingStatus.module.css";

const ClaimProcessingStatus = ({ percentage, isLoading }) => {
  const renderPercentageBar = () => {
    const emptyKeyPercentage = percentage ? parseFloat(percentage) : 0;
    const filledKeyPercentage = 100 - emptyKeyPercentage;

    return (
      <div className={styles.percentageContainer}>
        {isLoading ? (
          <div className={styles.loadingSpinner}>Processing claim status...</div>
        ) : (
          <div className={styles.splitPercentageBar}>
            {emptyKeyPercentage > 0 && (
              <div
                className={styles.emptyKeysSection}
                style={{ width: `${emptyKeyPercentage}%` }}
              >
                {emptyKeyPercentage > 10 && `${emptyKeyPercentage}% Empty`}
              </div>
            )}
            <div
              className={styles.filledKeysSection}
              style={{
                width: emptyKeyPercentage === 0 ? "100%" : `${filledKeyPercentage}%`,
              }}
            >
              {filledKeyPercentage > 10 && `${filledKeyPercentage}%`}
            </div>
          </div>
        )}
        <p className={styles.statusLabel}>
          {emptyKeyPercentage > 50 ? "Needs Attention" : "Looking Good!"}
        </p>
      </div>
    );
  };

  return (
    <div>
      <h3 className={styles.percentHead}>Claim Processing Status</h3>
      {renderPercentageBar()}
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

<ClaimProcessingStatus 
              percentage={percentage} 
              isLoading={isLoading} 
            />
