import React from 'react';
import styles from './Sidebar.module.css';

const Sidebar = ({ onSelectForm }) => {
    const handleFormSelect = (formName) => {
        onSelectForm(formName);
    };

    return (
        <div className={styles.sidebar}>
            <h3 className={styles.sidebarTitle}>Select a Form</h3>
            <ul className={styles.formList}>
                <li className={styles.formItem} onClick={() => handleFormSelect('PAYMENT_INSTRUCTION_FORM')}>
                    Payment Instruction Form
                </li>
                <li className={styles.formItem} onClick={() => handleFormSelect('PAYMENT_DETAILS')}>
                    Payment Details
                </li>
                <li className={styles.formItem} onClick={() => handleFormSelect('LOST_POLICY_FORM')}>
                    Lost Policy Form
                </li>
                <li className={styles.formItem} onClick={() => handleFormSelect('LOST_POLICY_FORM_WITNESSED_BY')}>
                    Witness Details
                </li>
            </ul>
        </div>
    );
};

export default Sidebar;


/* Sidebar.module.css */
.sidebar {
    width: 250px; /* Fixed width for the sidebar */
    background-color: #f4f4f4; /* Light background for contrast */
    padding: 20px; /* Padding around the content */
    border-right: 1px solid #e0e0e0; /* Subtle border for separation */
    box-shadow: 2px 0 5px rgba(0, 0, 0, 0.1); /* Shadow for depth */
    transition: background-color 0.3s ease; /* Smooth transition for hover effect */
}

.sidebar:hover {
    background-color: #eaeaea; /* Change background on hover */
}

.sidebarTitle {
    font-size: 1.5em; /* Larger font for title */
    margin-bottom: 15px; /* Spacing below the title */
    color: #333; /* Dark color for text */
}

.formList {
    list-style-type: none; /* Remove default list styling */
    padding: 0; /* Remove default padding */
}

.formItem {
    padding: 10px; /* Padding for list items */
    cursor: pointer; /* Cursor indicates clickability */
    border-radius: 4px; /* Rounded corners for items */
    transition: background-color 0.3s ease; /* Smooth transition for hover effect */
}

.formItem:hover {
    background-color: #d0e1f9; /* Highlight color on hover */
    color: #007bff; /* Change text color on hover */
}

.formItem:active {
    background-color: #b0c4de; /* Active state for feedback */
}



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



/* FormDisplay.module.css */
.formDisplay {
    flex: 1; /* Allow the form display to grow and fill space */
    padding: 20px; /* Padding around the form display */
    background-color: #ffffff; /* White background for the form */
    border-radius: 8px; /* Rounded corners for the form display */
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1); /* Subtle shadow for depth */
    margin-left: 20px; /* Margin to separate from the sidebar */
}

.loadingMessage,
.selectMessage {
    text-align: center; /* Center the messages */
    color: #777; /* Grey color for the messages */
    font-size: 1.2em; /* Slightly larger font for readability */
}

.formContainer {
    margin-top: 20px; /* Spacing above the form container */
}

.formHead {
    font-size: 1.8em; /* Larger font size for the form heading */
    color: #333; /* Dark color for better visibility */
    margin-bottom: 20px; /* Spacing below the heading */
}

.formGroup {
    display: flex; /* Use flexbox for layout */
    flex-direction: column; /* Stack items vertically */
}

.formField {
    margin-bottom: 15px; /* Spacing between fields */
}

.formLabel {
    font-weight: bold; /* Bold labels for emphasis */
    color: #555; /* Dark grey color for labels */
}

.formInput {
    margin-top: 5px; /* Spacing above the input */
    padding: 10px; /* Padding around the input */
    border: 1px solid #ccc; /* Light border for inputs */
    border-radius: 4px; /* Rounded corners for inputs */
    background-color: #f9f9f9; /* Light background for inputs */
    color: #333; /* Dark color for input text */
    word-break: break-word; /* Break long words to prevent overflow */
}
