return (
  <div className={styles.verifyMainContainer}>
    {/* Claim ID and Status Section */}
    <div className={styles.headerSection}>
      <div className={styles.claimIdDisplay}>
        <div className={styles.claimIdBadge}>
          <span>Claim ID</span>
          <h3>{recNum || 'N/A'}</h3>
        </div>
        <div className={styles.claimIdBadge}>
          <span>PS ID</span>
          <h3>{psid || 'N/A'}</h3>
        </div>
      </div>
      <div className={styles.statusSection}>
        <ClaimProcessingStatus percentage={emptyKeysPercentage} isLoading={loading} />
      </div>
    </div>

    {/* Main Content Layout */}
    <div className={styles.verifyContainer}>
      <div>
        <Chatbot />
      </div>

      {/* Left and Right Panels */}
      <div className={styles.rightLeft}>
        {/* Claim Summary */}
        <div className={styles.leftPanel}>
          <div className={styles.panelHeader}>
            <h3>Claim Summary</h3>
          </div>
          <div className={styles.panelContent}>
            <p>{Dsummary || 'No summary available'}</p>
          </div>
        </div>

        {/* Verification Summary */}
        <div className={styles.rightPanel}>
          <div className={styles.panelHeader}>
            <h3>Verification Summary</h3>
          </div>
          <div className={styles.panelContent}>
            <div className={styles.summarySection}>
              <h4>Detailed Recommendation</h4>
              <p>{recommendation || 'No detailed recommendation available'}</p>
            </div>
            <div className={styles.claimDetails}>
              <div className={styles.claimDetailItem}>
                <strong>Claim Type:</strong>
                <span>{summary?.claimType || 'Not specified'}</span>
              </div>
              <div className={styles.claimDetailItem}>
                <strong>Claim Status:</strong>
                <span>{summary?.claimStatus || 'Pending'}</span>
              </div>
            </div>
          </div>
        </div>
      </div>

      {/* Insights & VerificationDB (Inline Layout) */}
      <div className={styles.bottomPanel}>
        <div className={styles.insightsPanel}>
          <Insights claimType={summary?.claimType || 'GENERAL'} />
        </div>
        <div className={styles.verificationDBPanel}>
          <VerificationDB recNum={recNum} psid={psid} />
        </div>
      </div>
    </div>

    {/* Generate Email Button */}
    <div className={styles.generateEmailContainer}>
      <button 
        className={styles.generateEmailButton} 
        onClick={HandleEmail}
        disabled={!summary || !recommendation}
      >
        <FontAwesomeIcon icon={faEnvelope} className={styles.emailIcon} />
        Generate Email
      </button>
    </div>
  </div>
);





.rightLeft {
  display: flex;
  gap: 10px;
}

.leftPanel, .rightPanel, .insightsPanel, .verificationDBPanel {
  flex: 1;
  background-color: white;
  border-radius: 12px;
  border: 1px solid #e1e4e8;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
  overflow: hidden;
}

.panelHeader {
  background-color: #f1f5f9;
  padding: 15px 20px;
}

.panelHeader h3 {
  margin: 0;
  color: #2c3e50;
  font-size: 1.1rem;
}

.panelContent {
  padding: 20px;
}

.summarySection h4 {
  color: #0f5fdc;
  margin-bottom: 10px;
  padding-bottom: 5px;
}

.claimDetails {
  margin-top: 20px;
  background-color: #f9fafb;
  border-radius: 8px;
  padding: 15px;
}

.claimDetailItem {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
  padding-bottom: 10px;
  border-bottom: 1px solid #e2e8f0;
}

.claimDetailItem:last-child {
  border-bottom: none;
}

.claimDetailItem strong {
  color: #2c3e50;
}

.claimDetailItem span {
  color: #4a5568;
}

/* Bottom Panel (Insights & VerificationDB) */
.bottomPanel {
  display: flex;
  gap: 10px;
  margin-top: 20px;
}

.insightsPanel, .verificationDBPanel {
  flex: 1;
  background-color: white;
  border-radius: 12px;
  border: 1px solid #e1e4e8;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
  overflow: hidden;
  padding: 20px;
}
