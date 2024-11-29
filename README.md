<button
          className={styles.verifyButton}
          onClick={() =>
            handleVerify(
              clid,
              data?.total_extracted_data
                ? JSON.parse(data?.total_extracted_data).CLAIM_FORM_DETAILS?.CLAIM_FORM_DETAILED_SUMMARY
                : "No Summary Available",
              selectedPolicy
            )
          }
          disabled={!rows.length}
        >
          Verify
        </button>
