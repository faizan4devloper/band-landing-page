const renderFormData = () => {
    if (!formData) {
        return <p className={styles.loadingMessage}>Loading data...</p>;
    }

    return (
        <div className={styles.tableContainer}>
            <h2 className={styles.formHead}>{selectedForm.replace(/_/g, ' ')}</h2>

            <button className={styles.nextBtn} onClick={handleNextClick}>
                Next <FontAwesomeIcon icon={faChevronRight} className={styles.nextIcon} />
            </button>

            <table className={styles.dataTable}>
                <thead>
                    <tr>
                        <th className={styles.tableHeader}>Field</th>
                        <th className={styles.tableHeader}>Value</th>
                    </tr>
                </thead>
                <tbody>
                    {Object.entries(formData).map(([key, value]) => (
                        <tr key={key} className={styles.tableRow}>
                            <td className={styles.tableCell}>
                                {key.replace(/_/g, ' ')}
                            </td>
                            <td className={styles.tableCell}>
                                {/* Check if the value is an array */}
                                {Array.isArray(value) ? (
                                    <ul>
                                        {value.map((item, index) => (
                                            <li key={index}>{item}</li>
                                        ))}
                                    </ul>
                                ) : 
                                // Check if the value is an object
                                typeof value === 'object' && value !== null ? (
                                    <pre>{JSON.stringify(value, null, 2)}</pre>
                                ) : (
                                    value
                                )}
                            </td>
                        </tr>
                    ))}
                </tbody>
            </table>
        </div>
    );
};
