import React, { useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';
import styles from './PaymentInstructionForm.module.css';

const PaymentInstructionForm = ({ formData = {} }) => {
    // State to manage expand/collapse
    const [isExpanded, setIsExpanded] = useState(false);

    // Toggle function to change the state
    const toggleExpand = () => {
        setIsExpanded(!isExpanded);
    };

    return (
        <div className={styles.formContainer}>
            <div className={styles.heading}>
                <h3>Payment Instruction Form</h3>
                {/* Font Awesome icon for expand/collapse */}
                <FontAwesomeIcon
                    icon={isExpanded ? faChevronUp : faChevronDown}
                    className={styles.expandIcon}
                    onClick={toggleExpand}
                />
            </div>

            {/* Content is hidden when not expanded */}
            {isExpanded && (
                <div className={styles.content}>
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
    transition: all 0.3s ease;
}

.heading {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.heading h3 {
    color: #333;
    font-size: 1.6rem;
    border-bottom: 2px solid #7ca2e1;
    padding-bottom: 10px;
    margin-bottom: 20px;
}

.expandIcon {
    cursor: pointer;
    color: #7ca2e1;
    font-size: 1.4rem;
    transition: transform 0.3s ease;
}

.content {
    margin-top: 10px;
}

p {
    font-size: 1.1rem;
    margin: 10px 0;
    color: #555;
}

strong {
    color: #7ca2e1;
}
