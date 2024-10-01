import React from 'react';
import styles from './FormStyles.module.css'; // Import shared styles

const LostPolicyForm = ({ lostPolicyFormData = {} }) => {
    return (
        <div className={styles.formContainer}>
            <h3>Lost Policy Form</h3>
            {Object.keys(lostPolicyFormData).length > 0 ? (
                Object.entries(lostPolicyFormData).map(([key, value]) => (
                    <p key={key}>
                        <strong>{key.replace(/_/g, ' ')}:</strong> {value || 'N/A'}
                    </p>
                ))
            ) : (
                <p>No data available</p>
            )}
        </div>
    );
};

export default LostPolicyForm;

import React from 'react';
import styles from './FormStyles.module.css'; // Import shared styles

const PaymentDetails = ({ paymentData = {} }) => {
    return (
        <div className={styles.formContainer}>
            <h3>Payment Details Form</h3>
            {Object.keys(paymentData).length > 0 ? (
                Object.entries(paymentData).map(([key, value]) => (
                    <p key={key}>
                        <strong>{key.replace(/_/g, ' ')}:</strong> {value || 'N/A'}
                    </p>
                ))
            ) : (
                <p>No data available</p>
            )}
        </div>
    );
};

export default PaymentDetails;


import React from 'react';
import styles from './FormStyles.module.css'; // Import shared styles

const PaymentInstructionForm = ({ formData = {} }) => {
    return (
        <div className={styles.formContainer}>
            <h3>Payment Instruction Form</h3>
            {Object.keys(formData).length > 0 ? (
                Object.entries(formData).map(([key, value]) => (
                    <p key={key}>
                        <strong>{key.replace(/_/g, ' ')}:</strong> {value || 'N/A'}
                    </p>
                ))
            ) : (
                <p>No data available</p>
            )}
        </div>
    );
};

export default PaymentInstructionForm;


import React from 'react';
import styles from './FormStyles.module.css'; // Import shared styles

const WitnessDetails = ({ witnessData = {} }) => {
    return (
        <div className={styles.formContainer}>
            <h3>Witness Details</h3>
            {Object.keys(witnessData).length > 0 ? (
                Object.entries(witnessData).map(([key, value]) => (
                    <p key={key}>
                        <strong>{key.replace(/_/g, ' ')}:</strong> {value || 'N/A'}
                    </p>
                ))
            ) : (
                <p>No data available</p>
            )}
        </div>
    );
};

export default WitnessDetails;



/* General form container */
.formContainer {
    background-color: #ffffff;
    padding: 20px;
    margin: 20px 0;
    border-left: 5px solid #7ca2e1; /* Same border for all forms */
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    max-width: 600px; /* Uniform width for all forms */
    width: 100%; /* Ensure it fits within the parent container */
}

/* Headings for all forms */
.formContainer h3 {
    color: #333;
    font-size: 1.6rem;
    border-bottom: 2px solid #7ca2e1;
    padding-bottom: 10px;
    margin-bottom: 20px;
    font-weight: 600;
    text-align: left;
}

/* Paragraphs for displaying form data */
.formContainer p {
    font-size: 1.1rem;
    margin: 10px 0;
    color: #555;
    word-wrap: break-word; /* Handle long text gracefully */
}

/* Strong text to highlight labels */
strong {
    color: #7ca2e1;
    font-weight: 500;
}
