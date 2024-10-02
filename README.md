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
            <div className={styles.cardContainer}>
                <h2 className={styles.formHead}>{selectedForm.replace(/_/g, ' ')}</h2>
                <div className={styles.cardGroup}>
                    {Object.entries(selectedData).map(([key, value]) => (
                        <div key={key} className={styles.card}>
                            <div className={styles.cardHeader}>
                                <label className={styles.formLabel}>{key.replace(/_/g, ' ')}:</label>
                            </div>
                            <div className={styles.cardContent}>
                                <p className={styles.formInput}>{value}</p>
                            </div>
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


/* FormDisplay.module.css */
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

/* Loading message styling */
.loadingMessage,
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

/* Card container */
.cardContainer {
    display: flex;
    flex-direction: column;
    gap: 20px;
}

/* Card group styles */
.cardGroup {
    display: flex;
    flex-direction: column;
    gap: 15px;
}

/* Card styles */
.card {
    background-color: #ffffff; /* White background for card */
    border-radius: 10px; /* Rounded corners for cards */
    box-shadow: 0 2px 15px rgba(0, 0, 0, 0.1); /* Shadow for depth */
    padding: 15px; /* Padding inside the card */
    transition: transform 0.3s ease, box-shadow 0.3s ease; /* Smooth transition for hover effect */
}

/* Card hover effect */
.card:hover {
    transform: translateY(-5px); /* Lift effect */
    box-shadow: 0 5px 25px rgba(0, 0, 0, 0.2); /* Deeper shadow on hover */
}

/* Form label styles */
.formLabel {
    font-size: 1.2rem;
    color: #333; /* Dark grey for labels */
    font-weight: 600;
}

/* Form input field styles */
.formInput {
    padding: 10px;
    font-size: 1.1rem;
    color: #111827; /* Dark color for input text */
    background-color: #f1f5f9; /* Light background for input */
    border: 1px solid #e2e8f0; /* Light border */
    border-radius: 8px; /* Rounded corners */
    margin-top: 5px; /* Margin above input */
}

/* Button styles (if needed in future) */
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
