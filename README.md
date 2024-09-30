import React from 'react';
import styles from './Sidebar.module.css';
import { FaFileAlt, FaMoneyCheckAlt, FaFileContract, FaUserCheck } from 'react-icons/fa'; // Example icons

const Sidebar = ({ onSelectForm }) => {
    return (
        <div className={styles.sidebar}>
            <h3 className={styles.sidebarTitle}>Forms</h3>
            <ul className={styles.formList}>
                <li onClick={() => onSelectForm('PaymentInstructionForm')} className={styles.formItem}>
                    <FaFileAlt className={styles.icon} /> Payment Instruction Form
                </li>
                <li onClick={() => onSelectForm('PaymentDetails')} className={styles.formItem}>
                    <FaMoneyCheckAlt className={styles.icon} /> Payment Details
                </li>
                <li onClick={() => onSelectForm('LostPolicyForm')} className={styles.formItem}>
                    <FaFileContract className={styles.icon} /> Lost Policy Form
                </li>
                <li onClick={() => onSelectForm('WitnessDetails')} className={styles.formItem}>
                    <FaUserCheck className={styles.icon} /> Witness Details
                </li>
            </ul>
        </div>
    );
};

export default Sidebar;


.sidebar {
    flex: 1;
    background: linear-gradient(135deg, #6e8efb, #a777e3);
    padding: 30px;
    border-radius: 15px;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2);
    color: #fff;
    font-family: 'Poppins', sans-serif;
}

.sidebarTitle {
    font-size: 1.8rem;
    font-weight: bold;
    margin-bottom: 1.5rem;
    text-align: center;
    color: #ffffff;
}

.formList {
    list-style: none;
    padding: 0;
    margin: 0;
}

.formItem {
    cursor: pointer;
    display: flex;
    align-items: center;
    padding: 15px;
    margin-bottom: 15px;
    border-radius: 8px;
    transition: background 0.3s, transform 0.3s;
    background: rgba(255, 255, 255, 0.2);
}

.formItem:hover {
    background: rgba(255, 255, 255, 0.3);
    transform: translateY(-5px);
}

.icon {
    margin-right: 10px;
    font-size: 1.2rem;
    color: #f5f5f5;
}

.formItem:hover .icon {
    color: #ffffff;
}




.formContainer {
    background-color: #ffffff;
    padding: 30px;
    margin: 20px 0;
    border-left: 5px solid #7ca2e1;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    border-radius: 10px;
    max-width: 700px;
    transition: box-shadow 0.3s ease;
}

.formContainer:hover {
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.2);
}

h3 {
    color: #4a4a4a;
    font-size: 1.8rem;
    font-weight: 600;
    border-bottom: 2px solid #7ca2e1;
    padding-bottom: 10px;
    margin-bottom: 25px;
    font-family: 'Poppins', sans-serif;
}

p {
    font-size: 1.2rem;
    color: #555555;
    line-height: 1.6;
    margin: 12px 0;
}

strong {
    color: #7ca2e1;
    font-weight: 600;
}
