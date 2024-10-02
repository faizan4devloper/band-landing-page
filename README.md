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



/* Container styling */
.sidebar {
    background: rgba(0, 0, 0, 0.5);
    backdrop-filter: blur(10px);
box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);
    padding: 20px;
    border-radius: 12px;
    height: 100%; /* Full height as defined by parent */
    display: flex;
    flex-direction: column;
    gap: 15px; /* Space between items */
    overflow-y: auto; /* Allow scrolling if necessary */
}


/* Sidebar title */
.sidebarTitle {
    color: #ffffff; /* White title for contrast */
    font-size: .9rem;
    font-weight: bold;
    /*margin-bottom: 20px;*/
    margin:0px;
    text-align: center;
}

/* Form list */
.formList {
    list-style: none;
    padding: 0;
    margin: 0;
}

/* Form items */
.formItem {
    display: flex;
    align-items: center;
    padding: 12px;
    margin-bottom: 12px;
background: linear-gradient(135deg, #F2F2F2 -20%, #7ca2e1);
border-radius: 8px;
    color: #fff; /* Light text color */
    font-size: 1.1rem;
    cursor: pointer;
    transition: all 0.3s ease;
}

.formItem:hover {
    background: #617da8; /* Lighter on hover */
    transform: translateX(5px); /* Slight movement on hover */
}

.icon {
    margin-right: 10px;
    color: #ffffff; /* Light blue icon color */
    transition: color 0.3s;
}

.formItem:hover .icon {
    color: #fbbf24; /* Icon changes to gold on hover */
}

/* Active form item */
.active {
    background: #7588b2; /* Slightly lighter background for active state */
    color: #ffffff;
}

.active .icon {
    color: #fbbf24; /* Gold icon for active state */
}





// FormDisplay.js
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
background: rgba(0, 0, 0, 0.5);
    backdrop-filter: blur(10px);
    padding: 20px;
    border-radius: 12px;
box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);
height: 100%; /* Full height as defined by parent */
    overflow-y: auto; /* Scroll if form data exceeds container height */
    display: flex;
    flex-direction: column;
    gap: 20px; /* Space between form fields or content */
    width: 430px;
}

/* Form heading */
.formHead {
    font-size: 1.6rem;
    font-weight: bold;
    color: #1e293b;
    text-align: center;
    margin-bottom: 10px;
}

/* Form field group */
.formGroup {
    display: flex;
    flex-direction: column;
    gap: 10px; /* Space between input fields */
}

/* Form labels */
.formLabel {
    font-size: 1.2rem;
    color: #475569;
    font-weight: 500;
}

/* Form input field */
.formInput {
    padding: 12px;
    font-size: 1.1rem;
    color: #334155;
    background-color: #f1f5f9;
    border: 1px solid #e2e8f0;
    border-radius: 8px;
    transition: all 0.3s ease;
}

/* Form input field on focus */
.formInput:focus {
    border-color: #7ca2e1;
    outline: none;
    background-color: #ffffff;
    box-shadow: 0 0 0 3px rgba(124, 162, 225, 0.2);
}

/* Button styling */
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

/* Button hover */
.formButton:hover {
    background-color: #2563eb;
}

/* Button disabled state */
.formButton:disabled {
    background-color: #94a3b8;
    cursor: not-allowed;
}
.selectMessage{
    color: #ffffff;
    text-align: center;
}
