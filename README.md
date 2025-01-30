const renderPercentageBar = () => {
  // Parse the empty key percentage
  const emptyKeyPercentage = percentage ? parseFloat(percentage) : 0;
  const filledKeyPercentage = 100 - emptyKeyPercentage;

  return (
    <div className={styles.percentageContainer}>
      {isLoading ? (
        <div className={styles.loadingSpinner}>
          Processing claim status...
        </div>
      ) : (
        <div className={styles.splitPercentageBar}>
          <div 
            className={styles.emptyKeysSection}
            style={{ 
              width: `${emptyKeyPercentage}%`,
              backgroundColor: '#FF6B6B' // Vibrant Red
            }}
          >
            {emptyKeyPercentage}% Empty
          </div>
          <div 
            className={styles.filledKeysSection}
            style={{ 
              width: `${filledKeyPercentage}%`,
              backgroundColor: '#4ECB71' // Vibrant Green
            }}
          >
            {filledKeyPercentage}% Filled
          </div>
        </div>
      )}
    </div>
  );
};



.percentageContainer {
  width: 100%;
  height: 30px;
  background-color: #f0f0f0;
  border-radius: 15px;
  overflow: hidden;
  margin-top: 10px;
}

.splitPercentageBar {
  display: flex;
  width: 100%;
  height: 100%;
}

.emptyKeysSection {
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-weight: bold;
  transition: width 0.5s ease;
}

.filledKeysSection {
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-weight: bold;
  transition: width 0.5s ease;
}

/* Optional: Add a more detailed styling */
.statusLabel {
  font-size: 14px;
  margin-top: 10px;
  text-align: center;
  color: #333;
}
