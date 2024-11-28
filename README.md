<button
  className={styles.verifyButton}
  onClick={() => handleVerify(rows[0]?.recNum, data?.total_extracted_data && JSON.parse(data.total_extracted_data)?.CLAIM_FORM_DETAILS?.CLAIM_FORM_DETAILED_SUMMARY)}
  disabled={!rows.length}
>
  Verify
</button>
