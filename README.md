import React, { useEffect, useState } from 'react';
import formDataJson from './formData.json'; // Adjust the path as necessary
import styles from './FormDisplay.module.css';

const FormDisplay = ({ selectedForm }) => {
    const [formData, setFormData] = useState(null);

    useEffect(() => {
        // Simulate fetching data from a JSON file
        setFormData(formDataJson.extracted_data);
    }, []);

    // Render form data based on selected form
    const renderFormData = () => {
        if (!formData) {
            return <p className={styles.selectMessage}>Loading data...</p>;
        }

        const selectedData = formData[selectedForm];

        if (!selectedData) {
            return <p className={styles.selectMessage}>Please select a form to view its data.</p>;
        }

        return (
            <div>
                <h2 className={styles.formHead}>{selectedForm}</h2>
                <div className={styles.formGroup}>
                    {Object.entries(selectedData).map(([key, value]) => (
                        <div key={key} className={styles.formField}>
                            <label className={styles.formLabel}>{key.replace(/_/g, ' ')}:</label>
                            <p className={styles.formInput}>{value}</p>
                        </div>
                    ))}
                </div>
            </div>
        );
    };

    return (
        <div className={styles.formDisplay}>
            {renderFormData()}
        </div>
    );
};

export default FormDisplay;




.formDisplay {
    background: rgba(0, 0, 0, 0.5);
    backdrop-filter: blur(10px);
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);
    height: 100%; /* Full height as defined by parent */
    overflow-y: auto; /* Scroll if form data exceeds container height */
    display: flex;
    flex-direction: column;
    gap: 20px; /* Space between form fields or content */
}

.formHead {
    font-size: 1.6rem;
    font-weight: bold;
    color: #1e293b;
    text-align: center;
    margin-bottom: 10px;
}

.formGroup {
    display: flex;
    flex-direction: column;
    gap: 10px; /* Space between input fields */
}

.formField {
    display: flex;
    flex-direction: column;
}

.formLabel {
    font-size: 1.2rem;
    color: #475569;
    font-weight: 500;
}

.formInput {
    padding: 12px;
    font-size: 1.1rem;
    color: #334155;
    background-color: #f1f5f9;
    border: 1px solid #e2e8f0;
    border-radius: 8px;
}
.selectMessage {
    color: #ffffff;
    text-align: center;
}
