import React, { useState } from 'react';
import styles from './NewSidebar.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronRight } from '@fortawesome/free-solid-svg-icons';

const Sidebar = ({ onSelectForm }) => {
    const [activeItem, setActiveItem] = useState('')
    
    const handleFormSelect = (formName) => {
        setActiveItem(formName);
        onSelectForm(formName);
    };

    return (
        <div className={styles.sidebar}>
            <h3 className={styles.sidebarTitle}>Select a Form</h3>
            <ul className={styles.formList}>
                <li
                    className={`${styles.formItem} ${activeItem === 'claimassist-history-lambda' ? styles.active : ''}`}
                    onClick={() => handleFormSelect('claimassist-history-lambda')}
                >
                    <span className={styles.formText}>Verification Status</span>
                    <FontAwesomeIcon icon={faChevronRight} className={styles.chevronIcon} />
                </li>
                <li
                    className={`${styles.formItem} ${activeItem === 'PAYMENT_DETAILS' ? styles.active : ''}`}
                    onClick={() => handleFormSelect('PAYMENT_DETAILS')}
                >
                    <span className={styles.formText}>Reviewer Insights</span>
                    <FontAwesomeIcon icon={faChevronRight} className={styles.chevronIcon} />
                </li>
                <li
                    className={`${styles.formItem} ${activeItem === 'LOST_POLICY_FORM' ? styles.active : ''}`}
                    onClick={() => handleFormSelect('LOST_POLICY_FORM')}
                >
                    <span className={styles.formText}>Checklist</span>
                    <FontAwesomeIcon icon={faChevronRight} className={styles.chevronIcon} />
                </li>
                
            </ul>
        </div>
    );
};

export default Sidebar;




import React, { useEffect, useState } from 'react';
import { useNavigate } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronRight, faSignature, faIdCard, faFileAlt, faCalendarAlt, faQuestionCircle, faInfoCircle } from '@fortawesome/free-solid-svg-icons';
import formDataJson from '../../../Json/NewEntities.json';
import styles from './NewFormDisplay.module.css';

const NewFormDisplay = ({ selectedForm }) => {
    const [formData, setFormData] = useState(null);
    const navigate = useNavigate();

    useEffect(() => {
        setFormData(formDataJson); // Set the entire JSON structure for now
    }, []);

    const handleNextClick = () => {
        navigate('/New-page');
    };

    const chooseIcon = (key) => {
        switch (key.toLowerCase()) {
            case 'approver1_focuses_on':
                return faInfoCircle;
            case 'approver2_focuses_on':
                return faSignature;
            case 'additional_information':
                return faFileAlt;
            case 'suggested_action_items':
                return faQuestionCircle;
            case 'detailed_summary':
                return faIdCard;
            default:
                return faCalendarAlt;
        }
    };

    const renderFormData = () => {
        if (!formData) {
            return <p className={styles.loadingMessage}>Loading data...</p>;
        }

        const selectedData = formData[selectedForm];

        if (!selectedData) {
            return <p className={styles.selectMessage}>Please select a form to view its data.</p>;
        }

        return (
            <div className={styles.gridContainer}>
                {Object.entries(selectedData).map(([key, value]) => (
                    <div key={key} className={styles.card}>
                        <div className={styles.cardHeader}>
                            <FontAwesomeIcon icon={chooseIcon(key)} className={styles.cardIcon} />
                            {key.replace(/_/g, ' ')}
                        </div>
                        <div className={styles.cardContent}>
                            {Array.isArray(value) ? value.join(', ') : value}
                        </div>
                    </div>
                ))}

                
            </div>
        );
    };

    return <div className={styles.formDisplay}>{renderFormData()}</div>;
};

export default NewFormDisplay;
