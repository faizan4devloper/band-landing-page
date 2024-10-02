import React, { useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faFileInvoice, faCreditCard, faFileAlt, faUser } from '@fortawesome/free-solid-svg-icons';
import styles from './Sidebar.module.css';

const Sidebar = ({ onSelectForm }) => {
    const [activeItem, setActiveItem] = useState('');

    const handleSelect = (formName) => {
        setActiveItem(formName);
        onSelectForm(formName);
    };

    return (
        <div className={styles.sidebar}>
            <h3 className={styles.sidebarTitle}>Forms</h3>
            <ul className={styles.formList}>
                <li 
                    onClick={() => handleSelect('PaymentInstructionForm')} 
                    className={`${styles.formItem} ${activeItem === 'PaymentInstructionForm' ? styles.active : ''}`}
                >
                    <FontAwesomeIcon icon={faFileInvoice} className={styles.icon} />
                    Payment Instruction Form
                </li>
                <li 
                    onClick={() => handleSelect('PaymentDetails')} 
                    className={`${styles.formItem} ${activeItem === 'PaymentDetails' ? styles.active : ''}`}
                >
                    <FontAwesomeIcon icon={faCreditCard} className={styles.icon} />
                    Payment Details
                </li>
                <li 
                    onClick={() => handleSelect('LostPolicyForm')} 
                    className={`${styles.formItem} ${activeItem === 'LostPolicyForm' ? styles.active : ''}`}
                >
                    <FontAwesomeIcon icon={faFileAlt} className={styles.icon} />
                    Lost Policy Form
                </li>
                <li 
                    onClick={() => handleSelect('WitnessDetails')} 
                    className={`${styles.formItem} ${activeItem === 'WitnessDetails' ? styles.active : ''}`}
                >
                    <FontAwesomeIcon icon={faUser} className={styles.icon} />
                    Witness Details
                </li>
            </ul>
        </div>
    );
};

export default Sidebar;



/* Sidebar container */
.sidebar {
    background: linear-gradient(135deg, rgba(30, 41, 59, 0.85), rgba(51, 65, 85, 0.85));
    padding: 20px;
    border-radius: 12px;
    height: 100%;
    display: flex;
    flex-direction: column;
    gap: 20px;
    width: 250px;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);
}

/* Sidebar title */
.sidebarTitle {
    color: #fff;
    font-size: 1.2rem;
    font-weight: 700;
    text-align: center;
    margin-bottom: 10px;
}

/* Form list styles */
.formList {
    list-style: none;
    padding: 0;
    margin: 0;
    display: flex;
    flex-direction: column;
    gap: 15px;
}

/* Form item styles */
.formItem {
    display: flex;
    align-items: center;
    padding: 15px;
    background: linear-gradient(135deg, #4f709c, #7ca2e1);
    border-radius: 10px;
    color: #ffffff;
    font-size: 1.1rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s ease;
    gap: 10px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

/* Hover and active states */
.formItem:hover {
    background: #6f92c2;
    transform: translateX(5px);
    box-shadow: 0 6px 20px rgba(0, 0, 0, 0.15);
}

.icon {
    color: #ffffff;
    transition: color 0.3s;
}

/* Active item styles */
.active {
    background: #617da8;
    color: #fff;
}

.active .icon {
    color: #fbbf24;
}



import React from 'react';
import PaymentInstructionForm from '../Entities/PaymentInstructionForm';
import PaymentDetails from '../Entities/PaymentDetails';
import LostPolicyForm from '../Entities/LostPolicyForm';
import WitnessDetails from '../Entities/WitnessDetails';
import styles from './FormDisplay.module.css';

const FormDisplay = ({ selectedForm, formData, paymentData, lostPolicyFormData, witnessData }) => {
    const renderFormData = () => {
        switch (selectedForm) {
            case 'PaymentInstructionForm':
                return <PaymentInstructionForm formData={formData} />;
            case 'PaymentDetails':
                return <PaymentDetails paymentData={paymentData} />;
            case 'LostPolicyForm':
                return <LostPolicyForm lostPolicyFormData={lostPolicyFormData} />;
            case 'WitnessDetails':
                return <WitnessDetails witnessData={witnessData} />;
            default:
                return <p className={styles.selectMessage}>Please select a form to view its data.</p>;
        }
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
