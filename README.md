import React from "react";
import styles from "./ClaimProcessingStatus.module.css";

const ClaimProcessingStatus = ({ percentage, isLoading, dataExtracted }) => {
  const emptyKeyPercentage =
    percentage !== undefined ? Math.max(0, Math.min(100, parseFloat(percentage))) : 0;

  const filledKeyPercentage = 100 - emptyKeyPercentage;

  return (
    <div>
      <h3 className={styles.percentHead}>Claim Processing Status</h3>

      <div className={styles.percentageContainer}>
        <div className={styles.splitPercentageBar}>
          {!dataExtracted ? (
            /** Show animated loading effect while extraction is in progress */
            <div className={styles.loadingEffect}></div>
          ) : (
            /** Show the actual progress bar when extraction is done */
            <>
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
            </>
          )}
        </div>
      </div>
    </div>
  );
};

export default ClaimProcessingStatus;



.loadingEffect {
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, #e0e0e0 25%, #f0f0f0 50%, #e0e0e0 75%);
  background-size: 200% 100%;
  animation: loadingAnimation 1.5s infinite linear;
}

@keyframes loadingAnimation {
  0% {
    background-position: 200% 0;
  }
  100% {
    background-position: -200% 0;
  }
}


<ClaimProcessingStatus 
  percentage={percentage} 
  isLoading={isLoading} 
  dataExtracted={!!data}  // Only switch to percentage when extraction is done
/>
