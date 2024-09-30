import React, { useState } from 'react';
import sidebarStyles from './Sidebar.module.css';

const Sidebar = () => {
    const [selectedForm, setSelectedForm] = useState('');

    return (
        <div className={sidebarStyles.sidebar}>
            <h3 className={sidebarStyles.sidebarTitle}>Forms</h3>
            <ul className={sidebarStyles.formList}>
                <li
                    onClick={() => setSelectedForm('PaymentInstructionForm')}
                    className={`${sidebarStyles.formItem} ${selectedForm === 'PaymentInstructionForm' ? sidebarStyles.active : ''}`}
                >
                    Payment Instruction Form
                </li>
                <li
                    onClick={() => setSelectedForm('PaymentDetails')}
                    className={`${sidebarStyles.formItem} ${selectedForm === 'PaymentDetails' ? sidebarStyles.active : ''}`}
                >
                    Payment Details
                </li>
                <li
                    onClick={() => setSelectedForm('LostPolicyForm')}
                    className={`${sidebarStyles.formItem} ${selectedForm === 'LostPolicyForm' ? sidebarStyles.active : ''}`}
                >
                    Lost Policy Form
                </li>
                <li
                    onClick={() => setSelectedForm('WitnessDetails')}
                    className={`${sidebarStyles.formItem} ${selectedForm === 'WitnessDetails' ? sidebarStyles.active : ''}`}
                >
                    Witness Details
                </li>
            </ul>
        </div>
    );
};

export default Sidebar;





/* Sidebar.module.css */

.sidebar {
    flex: 1;
    background: linear-gradient(135deg, rgba(29, 82, 217, 0.9), rgba(2, 156, 178, 0.8));
    padding: 20px;
    border-radius: 15px;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.25);
    color: white;
    transition: all 0.3s ease-in-out;
    max-width: 300px;
    height: 100vh;
}

.sidebar:hover {
    box-shadow: 0 6px 30px rgba(0, 0, 0, 0.35);
}

.sidebarTitle {
    font-size: 1.8rem;
    font-weight: bold;
    text-align: center;
    margin-bottom: 20px;
    color: #fff;
    letter-spacing: 1px;
}

.formList {
    list-style: none;
    padding: 0;
    margin: 0;
}

.formItem {
    cursor: pointer;
    padding: 15px;
    margin-bottom: 15px;
    border-radius: 10px;
    font-size: 1.1rem;
    text-align: center;
    color: #fff;
    background: rgba(255, 255, 255, 0.1);
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
    transition: all 0.3s ease-in-out;
    font-family: 'Poppins', sans-serif;
}

.formItem:hover {
    background: rgba(255, 255, 255, 0.2);
    transform: scale(1.05);
    box-shadow: 0 6px 15px rgba(0, 0, 0, 0.3);
}

.formItem:active {
    transform: scale(0.98);
    background: rgba(255, 255, 255, 0.3);
}

.formItem.active {
    background: rgba(255, 255, 255, 0.4);
    font-weight: bold;
    box-shadow: 0 6px 15px rgba(0, 0, 0, 0.4);
}

@keyframes hoverEffect {
    0% {
        transform: scale(1);
    }
    50% {
        transform: scale(1.05);
    }
    100% {
        transform: scale(1);
    }
}
