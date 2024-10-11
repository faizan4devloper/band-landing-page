<div className={styles.formSelection}>
            <div
                className={`${styles.formItem} ${selectedForm === 'claimassist-history-lambda' ? styles.active : ''}`}
                onClick={() => handleFormSelect('claimassist-history-lambda')}
            >
                Verification Status
                <FontAwesomeIcon icon={faChevronRight} className={`${styles.chevronIcon} ${selectedForm === 'claimassist-history-lambda' ? styles.active : ''}`} />
            </div>
            <div
                className={`${styles.formItem} ${selectedForm === 'PAYMENT_DETAILS' ? styles.active : ''}`}
                onClick={() => handleFormSelect('PAYMENT_DETAILS')}
            >
                Reviewer Insights
                <FontAwesomeIcon icon={faChevronRight} className={`${styles.chevronIcon} ${selectedForm === 'PAYMENT_DETAILS' ? styles.active : ''}`} />
            </div>
            <div
                className={`${styles.formItem} ${selectedForm === 'LOST_POLICY_FORM' ? styles.active : ''}`}
                onClick={() => handleFormSelect('LOST_POLICY_FORM')}
            >
                Checklist
                <FontAwesomeIcon icon={faChevronRight} className={`${styles.chevronIcon} ${selectedForm === 'LOST_POLICY_FORM' ? styles.active : ''}`} />
            </div>
        </div>
