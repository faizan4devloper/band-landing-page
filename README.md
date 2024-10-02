import React from 'react';
import styles from './Sidebar.module.css';

const Sidebar = ({ onSelectForm }) => {
    const handleFormSelect = (formName) => {
        onSelectForm(formName);
    };

    return (
        <div className={styles.sidebar}>
            <h3>Select a Form</h3>
            <ul>
                <li onClick={() => handleFormSelect('PAYMENT_INSTRUCTION_FORM')}>Payment Instruction Form</li>
                <li onClick={() => handleFormSelect('PAYMENT_DETAILS')}>Payment Details</li>
                <li onClick={() => handleFormSelect('LOST_POLICY_FORM')}>Lost Policy Form</li>
                <li onClick={() => handleFormSelect('LOST_POLICY_FORM_WITNESSED_BY')}>Witness Details</li>
            </ul>
        </div>
    );
};

export default Sidebar;


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
