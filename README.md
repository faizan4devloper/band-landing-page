import React from 'react';
import styles from './PaymentInstructionForm.module.css';

const PaymentInstructionForm = ({ formData = {} }) => {
    // Dynamically generate form fields from the formData object
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
