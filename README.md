 <div className={styles.container}>
      <div className={styles.leftWindow}>
        <div className={styles.claimSummary}>
          <h2>Claim Summary</h2>
          <p>Claim ID: CL1234567</p>
          <p>Reference Number: PS391481</p>
          <p>Task Type: FETCH_DRAFT_EMAIL</p>
        </div>
        <div className={styles.recommendations}>
          <h2>Recommendations</h2>
          <p>Some recommendation content goes here.</p>
        </div>
      </div>
      <div className={styles.rightWindow}>
        <div className={styles.draftEmail}>
          <h2>Draft Email</h2>
          {draftEmail ? (
            <textarea
              value={draftEmail}
              readOnly
              className={styles.emailBody}
            />
          ) : (
            <p>No draft email yet. Click "Generate Email" to fetch one.</p>
          )}
        </div>
        <div className={styles.llmResponse}>
          <h2>LLM Response</h2>
          <p>Generated responses will appear here.</p>
          <button
            onClick={handleGenerateEmail}
            disabled={loading}
            className={styles.generateButton}
          >
            {loading ? "Generating..." : "Generate Email"}
          </button>
        </div>
      </div>
    </div>
