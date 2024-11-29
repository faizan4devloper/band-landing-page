/* Container */
.verifyContainer {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
  padding: 20px;
  max-width: 1200px;
  margin: 0 auto;
}

/* Panels */
.leftPanel,
.rightPanel {
  padding: 20px;
  background-color: #ffffff;
  border-radius: 12px;
  box-shadow: 0 6px 10px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.leftPanel:hover,
.rightPanel:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.15);
}

.leftPanel h3,
.rightPanel h3 {
  font-size: 20px;
  color: #333333;
}

.leftPanel p,
.rightPanel p {
  font-size: 15px;
  line-height: 1.6;
  color: #666666;
}

/* Claim ID */
.claimIdDisplay {
  margin: 10px auto;
  font-size: 18px;
  font-weight: bold;
  text-align: center;
  color: #444444;
}

/* Spinner */
.spinnerContainer {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

/* Button */
.generateEmailButton {
  padding: 12px 24px;
  background: linear-gradient(90deg, #4caf50, #66bb6a, #4caf50);
  color: #ffffff;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 16px;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
}

.generateEmailButton:hover {
  transform: scale(1.05);
  box-shadow: 0 5px 10px rgba(76, 175, 80, 0.5);
}

/* Error Message */
.errorMessage {
  text-align: center;
  color: #ff5722;
  font-weight: bold;
  font-size: 18px;
}




import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faEnvelope } from "@fortawesome/free-solid-svg-icons";

return (
  <div>
    {/* Claim ID Display */}
    <div className={styles.claimIdDisplay}>
      <h3>Claim ID: {recNum}</h3>
    </div>

    <div className={styles.verifyContainer}>
      {/* Left Panel */}
      <div className={styles.leftPanel}>
        <h3>Claim Summary</h3>
        <p><strong>Summary:</strong> {Dsummary}</p>
        <p><strong>Claim Type:</strong> {summary.claimType}</p>
        <p><strong>Claim Status:</strong> {summary.claimStatus}</p>
      </div>

      {/* Right Panel */}
      <div className={styles.rightPanel}>
        <h3>Detailed Summary</h3>
        <p>{recommendation}</p>
      </div>
    </div>

    {/* Generate Email Button */}
    <div className={styles.genrateEmailContainer}>
      <button className={styles.generateEmailButton} onClick={HandleEmail}>
        <FontAwesomeIcon icon={faEnvelope} />
        Generate Email
      </button>
    </div>
  </div>
);
