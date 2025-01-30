const renderPercentageBar = () => {
  const emptyKeyPercentage = percentage ? parseFloat(percentage) : 0;
  const filledKeyPercentage = 100 - emptyKeyPercentage;

  return (
    <div className={styles.percentageContainer}>
      {isLoading ? (
        <div className={styles.loadingSpinner}>Processing claim status...</div>
      ) : (
        <div className={styles.splitPercentageBar}>
          <div
            className={styles.emptyKeysSection}
            style={{ width: `${emptyKeyPercentage}%` }}
          >
            {emptyKeyPercentage > 10 && `${emptyKeyPercentage}% Empty`}
          </div>
          <div
            className={styles.filledKeysSection}
            style={{ width: `${filledKeyPercentage}%` }}
          >
            {filledKeyPercentage > 10 && `${filledKeyPercentage}% Filled`}
          </div>
        </div>
      )}
      <p className={styles.statusLabel}>
        {emptyKeyPercentage > 50 ? "Needs Attention" : "Looking Good!"}
      </p>
    </div>
  );
};



:root {
  --empty-color: #ff6b6b; /* Vibrant Red */
  --filled-color: #4ecb71; /* Vibrant Green */
  --bg-color: #f0f0f0;
  --text-color: #ffffff;
}

.percentageContainer {
  width: 100%;
  height: 35px;
  background-color: var(--bg-color);
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
  color: var(--text-color);
  font-weight: 600;
  font-size: 14px;
  min-width: 12%; /* Prevents text from getting cut off */
  transition: width 0.6s ease-in-out;
  padding: 0 8px;
  white-space: nowrap;
}

.emptyKeysSection {
  background-color: var(--empty-color);
}

.filledKeysSection {
  background-color: var(--filled-color);
}

.statusLabel {
  font-size: 14px;
  margin-top: 8px;
  text-align: center;
  color: #444;
  font-weight: 500;
}









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
