<div className={`${styles.sectionWindow} ${styles.draftEmail}`}>
        <button
          className={styles.fetchButton}
          onClick={handleFetchDraftEmail}
          disabled={draftLoading}
        >
          {draftLoading ? "Fetching Draft..." : "Fetch Draft Email"}
        </button>

        {/* Display the draft email content */}
        {draftEmail && (
          <pre className={styles.emailContent}>{draftEmail}</pre>
        )}

        {/* Display error message if any */}
        {draftError && <p className={styles.errorText}>{draftError}</p>}
      </div>
