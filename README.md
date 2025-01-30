
<div className={styles.mainContentWrapper}>
  <div className={styles.claimIdDisplay}>
    {rows.length > 0 && <h3>Claim ID: {rows[0].recNum}</h3>}
  </div>

  <div className={styles.mainContentGrid}>
    {/* Left Column */}
    <div className={styles.leftColumn}>
      <div className="card">
        <DocumentPreview staticPreviewUrl={staticPreviewUrl} />
      </div>
      <div className="card">
        <ClaimClassification data={data} />
      </div>
    </div>

    {/* Right Column */}
    <div className={styles.rightColumn}>
      <div className="card">
        <ExtractedContent data={data} loading={loading} rows={rows} handleReload={handleReload} />
      </div>
      <div className={styles.verifyButtonContainer}>
        <button className={styles.verifyButton} onClick={handleVerifyClick} disabled={!rows.length}>
          Verify Claim <FontAwesomeIcon icon={faChevronRight} />
        </button>
      </div>
    </div>
  </div>
</div>






.mainContentGrid {
  display: grid;
  grid-template-columns: 1.5fr 1fr;
  gap: 24px;
  align-items: start;
}

.leftColumn, .rightColumn {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

/* Cards (Consistent Styling) */
.card {
  background: #ffffff;
  border-radius: 16px;
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.08);
  padding: 20px;
  transition: all 0.3s ease;
}

.card:hover {
  box-shadow: 0 10px 18px rgba(0, 0, 0, 0.1);
  transform: translateY(-3px);
}
