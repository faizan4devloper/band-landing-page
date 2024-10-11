<div
    className={`${styles.formItem} ${selectedForm === 'claimassist-history-lambda' ? styles.active : ''}`}
    onClick={() => handleFormSelect('claimassist-history-lambda')}
>
    Verification Status
    <FontAwesomeIcon icon={faChevronRight} className={`${styles.chevronIcon} ${selectedForm === 'claimassist-history-lambda' ? styles.activeIcon : ''}`} />
</div>


.chevronIcon {
    color: #f8fafc; /* Default color */
    transition: transform 0.3s ease;
    margin-left: 10px;
}

.formItem.active .chevronIcon {
    color: #2563eb; /* Active color when parent .formItem is active */
}
