const renderFormData = (formKey) => {
    if (!formData || !formData[formKey]) return null;

    const selectedData = formData[formKey];

    return (
        <div className={styles.formDataSection}>
            {Object.entries(selectedData).map(([key, value]) => {
                // Check if the value is an object
                const isObject = typeof value === 'object' && value !== null;

                return (
                    <div key={key}>
                        <h2 className={styles.formDataTitle}>
                            <FontAwesomeIcon icon={chooseIcon(key)} className={styles.cardIcon} />
                            {key.replace(/_/g, ' ')}
                        </h2>
                        <div className={styles.formDataContent}>
                            {/* If it's an object, use JSON.stringify or render its properties */}
                            {isObject ? (
                                <pre>{JSON.stringify(value, null, 2)}</pre>
                            ) : (
                                value // Render the value if it's not an object
                            )}
                        </div>
                    </div>
                );
            })}
        </div>
    );
};
