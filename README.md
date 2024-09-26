import React, { useState } from 'react';
import styles from './PaymentInstructionForm.module.css';

const PaymentInstructionForm = ({ formData = {} }) => {
    // State to control the dropdown visibility
    const [isOpen, setIsOpen] = useState(false);

    // Function to toggle the dropdown visibility
    const toggleDropdown = () => {
        setIsOpen(!isOpen);
    };

    return (
        <div className={styles.formContainer}>
            <div className={styles.header} onClick={toggleDropdown}>
                <h3>Payment Instruction Form</h3>
                {/* Arrow icon to indicate open/close state */}
                <span className={`${styles.arrowIcon} ${isOpen ? styles.open : ''}`}>&#9660;</span>
            </div>
            {/* Drop-down content */}
            <div className={`${styles.content} ${isOpen ? styles.open : ''}`}>
                <p><strong>Statement Date:</strong> {formData.STATEMENT_DATE || 'N/A'}</p>
                <p><strong>Policy Number:</strong> {formData.POLICY_NUMBER || 'N/A'}</p>
                <p><strong>Policy On the Life Of:</strong> {formData.POLICY_ON_THE_LIFE_OF || 'N/A'}</p>
                <p><strong>Policy Owner:</strong> {formData.POLICY_OWNER || 'N/A'}</p>
            </div>
        </div>
    );
};

export default PaymentInstructionForm;


.formContainer {
    background-color: #ffffff;
    padding: 20px;
    margin: 20px 0;
    border-radius: 10px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    max-width: 600px;
    transition: box-shadow 0.3s ease;
}

.formContainer:hover {
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.2);
}

/* Header for the form, clickable to toggle content */
.header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    cursor: pointer;
    padding: 10px;
    background-color: #7ca2e1;
    color: white;
    border-radius: 10px;
}

h3 {
    margin: 0;
    font-size: 1.6rem;
}

/* Arrow icon for indicating dropdown state */
.arrowIcon {
    font-size: 1.5rem;
    transition: transform 0.3s ease;
}

.arrowIcon.open {
    transform: rotate(180deg);
}

/* Content that will be toggled open/closed */
.content {
    max-height: 0;
    overflow: hidden;
    transition: max-height 0.4s ease-out;
}

.content.open {
    max-height: 500px; /* Adjust based on content */
}

/* Styling for the form fields */
p {
    font-size: 1.1rem;
    margin: 10px 0;
    color: #555;
    opacity: 0;
    transition: opacity 0.3s ease-out;
}

.content.open p {
    opacity: 1;
}

strong {
    color: #7ca2e1;
}
