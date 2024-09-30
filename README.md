// Sidebar.js
import React from 'react';
import styles from './Sidebar.module.css';

const Sidebar = ({ onSelectForm }) => {
    return (
        <div className={styles.sidebar}>
            <h3 className={styles.sidebarTitle}>Forms</h3>
            <ul className={styles.formList}>
                <li onClick={() => onSelectForm('PaymentInstructionForm')} className={styles.formItem}>Payment Instruction Form</li>
                <li onClick={() => onSelectForm('PaymentDetails')} className={styles.formItem}>Payment Details</li>
                <li onClick={() => onSelectForm('LostPolicyForm')} className={styles.formItem}>Lost Policy Form</li>
                <li onClick={() => onSelectForm('WitnessDetails')} className={styles.formItem}>Witness Details</li>
            </ul>
        </div>
    );
};

export default Sidebar;



.sidebar {
    flex: 1; /* Smaller than the upload section */
    background: rgba(0, 0, 0, 0.5);
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);
}

.sidebarTitle {
    font-size: 1.6rem;
    font-weight: bold;
    margin-bottom: 1rem;
}

.formList {
    list-style: none;
    padding: 0;
}

.formItem {
    cursor: pointer;
    padding: 10px;
    margin-bottom: 10px;
    border-radius: 6px;
    transition: background 0.3s;
    background: #e8f0fe; /* Light blue background */
}

.formItem:hover {
    background: #d1e7fd; /* Slightly darker blue on hover */
}
