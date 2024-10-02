import React from 'react';
import styles from './Sidebar.module.css';

const Sidebar = ({ onSelectForm }) => {
    const handleFormSelect = (formName) => {
        onSelectForm(formName);
    };

    return (
        <div className={styles.sidebar}>
            <h3 className={styles.sidebarTitle}>Select a Form</h3>
            <ul className={styles.formList}>
                <li className={styles.formItem} onClick={() => handleFormSelect('PAYMENT_INSTRUCTION_FORM')}>
                    Payment Instruction Form
                </li>
                <li className={styles.formItem} onClick={() => handleFormSelect('PAYMENT_DETAILS')}>
                    Payment Details
                </li>
                <li className={styles.formItem} onClick={() => handleFormSelect('LOST_POLICY_FORM')}>
                    Lost Policy Form
                </li>
                <li className={styles.formItem} onClick={() => handleFormSelect('LOST_POLICY_FORM_WITNESSED_BY')}>
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

