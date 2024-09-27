import React, { useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';
import styles from './PaymentInstructionForm.module.css';

const PaymentInstructionForm = ({ formData = {} }) => {
    const [isExpanded, setIsExpanded] = useState(true);

    const toggleExpand = () => {
        setIsExpanded(!isExpanded);
    };

    return (
        <div className={styles.formContainer}>
            <div className={styles.header} onClick={toggleExpand}>
                <h3>Payment Instruction Form</h3>
                <FontAwesomeIcon icon={isExpanded ? faChevronUp : faChevronDown} className={styles.icon} />
            </div>
            {isExpanded && (
                <div className={styles.formContent}>
                    <p><strong>Statement Date:</strong> {formData.STATEMENT_DATE || 'N/A'}</p>
                    <p><strong>Policy Number:</strong> {formData.POLICY_NUMBER || 'N/A'}</p>
                    <p><strong>Policy On the Life Of:</strong> {formData.POLICY_ON_THE_LIFE_OF || 'N/A'}</p>
                    <p><strong>Policy Owner:</strong> {formData.POLICY_OWNER || 'N/A'}</p>
                </div>
            )}
        </div>
    );
};

export default PaymentInstructionForm;


.formContainer {
    background-color: #ffffff;
    padding: 20px;
    margin: 20px 0;
    border-left: 5px solid #7ca2e1;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    max-width: 600px;
    transition: all 0.3s ease-in-out;
}

.header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    cursor: pointer;
    border-bottom: 2px solid #7ca2e1;
    padding-bottom: 10px;
    margin-bottom: 20px;
}

.icon {
    font-size: 1.2rem;
    color: #7ca2e1;
    transition: transform 0.3s ease;
}

.formContent {
    padding: 10px 0;
}

p {
    font-size: 1.1rem;
    margin: 10px 0;
    color: #555;
}

strong {
    color: #7ca2e1;
}
