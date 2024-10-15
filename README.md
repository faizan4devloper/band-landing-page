const renderFormData = (formKey) => {
    if (!formData || !formData[formKey]) return null;

    const selectedData = formData[formKey];

    return (
        <div className={styles.formDataSection}>
            {Object.entries(selectedData).map(([key, value]) => {
                // Handle case where value is undefined or null
                const safeValue = value !== undefined && value !== null ? value : 'No data available';

                return (
                    <div key={key}>
                        <h2 className={styles.formDataTitle}>
                            <FontAwesomeIcon icon={chooseIcon(key)} className={styles.cardIcon} />
                            {key.replace(/_/g, ' ')}
                        </h2>
                        {/* Safe rendering of content */}
                        <div className={styles.formDataContent}>
                            {Array.isArray(safeValue) ? safeValue.join(', ') : safeValue}
                        </div>
                    </div>
                );
            })}
        </div>
    );
};
