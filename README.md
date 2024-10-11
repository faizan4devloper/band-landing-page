import React, { useEffect, useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronRight, faSignature, faIdCard, faFileAlt, faCalendarAlt, faQuestionCircle, faInfoCircle } from '@fortawesome/free-solid-svg-icons';
import formDataJson from '../../../Json/NewEntities.json';
import styles from './NewFormDisplay.module.css';

const NewFormDisplay = () => {
    const [formData, setFormData] = useState(null);
    const [selectedForm, setSelectedForm] = useState('');

    useEffect(() => {
        setFormData(formDataJson);
    }, []);

    const handleFormSelect = (formName) => {
        setSelectedForm(formName);
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
            <div className={styles.sectionContainer}>
                {Object.entries(selectedData).map(([key, value]) => (
                    <div key={key} className={styles.expandableSection}>
                        <div className={styles.sectionHeader}>
                            <FontAwesomeIcon icon={chooseIcon(key)} className={styles.sectionIcon} />
                            <h3>{key.replace(/_/g, ' ')}</h3>
                        </div>
                        <div className={styles.sectionContent}>
                            {Array.isArray(value) ? value.join(', ') : value}
                        </div>
                    </div>
                ))}
            </div>
        );
    };

    return (
        <div className={styles.formDisplay}>
            <div className={styles.formSelection}>
                <div
                    className={`${styles.formItem} ${selectedForm === 'claimassist-history-lambda' ? styles.active : ''}`}
                    onClick={() => handleFormSelect('claimassist-history-lambda')}
                >
                    Verification Status
                    <FontAwesomeIcon icon={faChevronRight} className={styles.chevronIcon} />
                </div>
                <div
                    className={`${styles.formItem} ${selectedForm === 'PAYMENT_DETAILS' ? styles.active : ''}`}
                    onClick={() => handleFormSelect('PAYMENT_DETAILS')}
                >
                    Reviewer Insights
                    <FontAwesomeIcon icon={faChevronRight} className={styles.chevronIcon} />
                </div>
                <div
                    className={`${styles.formItem} ${selectedForm === 'LOST_POLICY_FORM' ? styles.active : ''}`}
                    onClick={() => handleFormSelect('LOST_POLICY_FORM')}
                >
                    Checklist
                    <FontAwesomeIcon icon={faChevronRight} className={styles.chevronIcon} />
                </div>
            </div>
            {renderFormData()}
        </div>
    );
};

export default NewFormDisplay;





@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap');

.formDisplay {
    background: linear-gradient(135deg, #1e293b, #334155);
    padding: 30px;
    height: 100%;
    width: 100%;
    overflow-y: auto;
    font-family: 'Poppins', sans-serif;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 30px;
}

/* Form selection section */
.formSelection {
    display: flex;
    gap: 15px;
    justify-content: center;
    flex-wrap: wrap;
    padding-bottom: 20px;
}

.formItem {
    background-color: #1f2937;
    color: #f8fafc;
    padding: 12px 20px;
    border-radius: 0px 0px 8px 8px;
    font-size: 1rem;
    font-weight: bold;
    text-transform: uppercase;
    cursor: pointer;
    transition: background 0.3s ease, transform 0.4s ease;
    display: flex;
    align-items: center;
    gap: 10px;
}

.formItem.active {
    background: linear-gradient(135deg, #F2F2F2 -20%, #7ca2e1);
}

.chevronIcon {
    color: #f8fafc;
    transition: transform 0.3s ease;
}

.formItem:hover .chevronIcon {
    transform: translateX(5px);
}

/* Expandable sections */
.sectionContainer {
    display: flex;
    flex-direction: column;
    gap: 20px;
    width: 100%;
}

.expandableSection {
    background-color: #f1f5f9;
    border-radius: 10px;
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
    overflow: hidden;
    transition: max-height 0.5s ease, padding 0.5s ease;
    padding: 20px;
    max-height: 500px; /* Adjust this based on content */
}

.sectionHeader {
    background: linear-gradient(135deg, #F2F2F2 -20%, #7ca2e1);
    color: white;
    padding: 15px;
    font-size: 1.5rem;
    font-weight: 600;
    display: flex;
    align-items: center;
    gap: 10px;
    cursor: pointer;
}

.sectionIcon {
    color: white;
}

.sectionContent {
    padding: 10px 15px;
    font-size: 1rem;
    color: #374151;
    line-height: 1.6;
    max-height: 0;
    overflow: hidden;
    transition: max-height 0.5s ease;
}

.expandableSection:hover .sectionContent {
    max-height: 1000px; /* Will stretch to fit content */
    padding: 20px;
}

/* Loading and select messages */
.loadingMessage,
.selectMessage {
    font-size: 1.5rem;
    color: #e2e8f0;
    text-align: center;
    margin-top: 20px;
}

/* Responsive design */
@media (max-width: 768px) {
    .formDisplay {
        padding: 15px;
    }

    .sectionHeader {
        font-size: 1.2rem;
    }
}
