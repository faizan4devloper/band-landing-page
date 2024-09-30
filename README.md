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
