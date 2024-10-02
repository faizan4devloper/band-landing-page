import React, { useEffect, useState } from 'react';
import formDataJson from '../../../Json/DocumentEntities.json'; // Import the JSON file
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
            return <p className={styles.loadingMessage}>Loading data...</p>;
        }

        const selectedData = formData[selectedForm];

        if (!selectedData) {
            return <p className={styles.selectMessage}>Please select a form to view its data.</p>;
        }

        return (
            <div className={styles.formContainer}>
                <h2 className={styles.formHead}>{selectedForm.replace(/_/g, ' ')}</h2>
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

/* Form display container */
.formDisplay {
    background: linear-gradient(135deg, rgba(30, 41, 59, 0.85), rgba(51, 65, 85, 0.85));
    padding: 25px;
    border-radius: 12px;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);
    height: 100%;
    width: 100%;
    overflow-y: auto;
    display: flex;
    flex-direction: column;
    gap: 20px;
}

/* Default message styling */
.selectMessage {
    font-size: 1.2rem;
    color: #ffffff;
    text-align: center;
    margin-top: 20px;
}

/* Form heading */
.formHead {
    font-size: 1.8rem;
    font-weight: bold;
    color: #ffffff;
    text-align: center;
    margin-bottom: 20px;
}

/* Form group styles */
.formGroup {
    display: flex;
    flex-direction: column;
    gap: 12px;
}

/* Form label styles */
.formLabel {
    font-size: 1.2rem;
    color: #d1d5db;
    font-weight: 600;
}

/* Form input field */
.formInput {
    padding: 12px;
    font-size: 1.1rem;
    color: #111827;
    background-color: #f1f5f9;
    border: 1px solid #e2e8f0;
    border-radius: 8px;
    transition: all 0.3s ease;
}

/* Input field on focus */
.formInput:focus {
    border-color: #7ca2e1;
    background-color: #ffffff;
    box-shadow: 0 0 0 3px rgba(124, 162, 225, 0.2);
}

/* Button styles */
.formButton {
    padding: 12px 20px;
    font-size: 1.2rem;
    font-weight: bold;
    color: #ffffff;
    background-color: #3b82f6;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    transition: background-color 0.3s ease;
    margin-top: 10px;
}

.formButton:hover {
    background-color: #2563eb;
}

.formButton:disabled {
    background-color: #94a3b8;
    cursor: not-allowed;
}
