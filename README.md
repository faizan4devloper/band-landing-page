import React, { useState } from 'react';
import styles from './NewSidebar.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronRight } from '@fortawesome/free-solid-svg-icons';

const Sidebar = ({ onSelectForm }) => {
    const [activeItem, setActiveItem] = useState('');

    const handleFormSelect = (formName) => {
        setActiveItem(formName);
        onSelectForm(formName); // Pass the form name to parent component
    };

    return (
        <div className={styles.sidebar}>
            <h3 className={styles.sidebarTitle}>Select a Form</h3>
            <ul className={styles.formList}>
                <li
                    className={`${styles.formItem} ${activeItem === 'PAYMENT_INSTRUCTION_FORM' ? styles.active : ''}`}
                    onClick={() => handleFormSelect('PAYMENT_INSTRUCTION_FORM')}
                >
                    <span className={styles.formText}>Payment Instruction Form</span>
                    <FontAwesomeIcon icon={faChevronRight} className={styles.chevronIcon} />
                </li>
                <li
                    className={`${styles.formItem} ${activeItem === 'PAYMENT_DETAILS' ? styles.active : ''}`}
                    onClick={() => handleFormSelect('PAYMENT_DETAILS')}
                >
                    <span className={styles.formText}>Payment Details</span>
                    <FontAwesomeIcon icon={faChevronRight} className={styles.chevronIcon} />
                </li>
                <li
                    className={`${styles.formItem} ${activeItem === 'LOST_POLICY_FORM' ? styles.active : ''}`}
                    onClick={() => handleFormSelect('LOST_POLICY_FORM')}
                >
                    <span className={styles.formText}>Lost Policy Form</span>
                    <FontAwesomeIcon icon={faChevronRight} className={styles.chevronIcon} />
                </li>
            </ul>
        </div>
    );
};

export default Sidebar;
