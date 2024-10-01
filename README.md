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
                return <p>Please select a form to view its data.</p>;
        }
    };

    return (
        <div className={styles.formDisplay}>
            {renderFormData()}
        </div>
    );
};

export default FormDisplay;



/* Main container for the form display */
.formDisplay {
    background-color: #f8fafc; /* Light background for contrast */
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1); /* Soft shadow for depth */
    max-width: 1000px; /* Set a maximum width for better readability */
    margin: 20px auto; /* Center the form display */
    transition: all 0.3s ease; /* Smooth transitions for any future changes */
}

/* Style the forms inside the formDisplay */
.formDisplay > * {
    margin-bottom: 20px; /* Add space between form elements */
}

/* When no form is selected */
.formDisplay p {
    color: #64748b; /* Muted blue-grey for the placeholder text */
    text-align: center;
    font-size: 1.2rem;
    padding: 40px 0;
}

/* Responsive behavior for smaller screens */
@media (max-width: 768px) {
    .formDisplay {
        padding: 15px;
        max-width: 90%; /* Allow the form to scale down on smaller screens */
    }

    .formDisplay p {
        font-size: 1rem; /* Adjust font size for readability on small devices */
    }
}


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
    flex: 1;
    background: #1e293b; /* Darker background for better contrast */
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 12px 30px rgba(0, 0, 0, 0.4); /* Stronger shadow for depth */
    transition: all 0.3s ease;
}

.sidebar:hover {
    transform: scale(1.02); /* Subtle scaling on hover */
}

/* Sidebar title */
.sidebarTitle {
    color: #ffffff; /* White title for contrast */
    font-size: 1.8rem;
    font-weight: bold;
    margin-bottom: 20px;
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
    padding: 15px;
    margin-bottom: 12px;
    background: #334155; /* Darker item background */
    border-radius: 8px;
    color: #e2e8f0; /* Light text color */
    font-size: 1.2rem;
    cursor: pointer;
    transition: all 0.3s ease;
}

.formItem:hover {
    background: #475569; /* Lighter on hover */
    transform: translateX(5px); /* Slight movement on hover */
}

.icon {
    margin-right: 10px;
    color: #94a3b8; /* Light blue icon color */
    transition: color 0.3s;
}

.formItem:hover .icon {
    color: #fbbf24; /* Icon changes to gold on hover */
}

/* Active form item */
.active {
    background: #64748b; /* Different background for active state */
    color: #ffffff;
}

.active .icon {
    color: #fbbf24; /* Gold icon for active state */
}
