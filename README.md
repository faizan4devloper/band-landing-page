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
            <div className={styles.formDataSection}>
                {Object.entries(selectedData).map(([key, value]) => (
                    <div key={key}>
                        <h2 className={styles.formDataTitle}>
                            <FontAwesomeIcon icon={chooseIcon(key)} className={styles.cardIcon} />
                            {key.replace(/_/g, ' ')}
                        </h2>
                        <p className={styles.formDataContent}>
                            {Array.isArray(value) ? value.join(', ') : value}
                        </p>
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
                <FontAwesomeIcon icon={faChevronRight} className={`${styles.chevronIcon} ${selectedForm === 'claimassist-history-lambda' ? styles.active : ''}`} />
            </div>
            <div
                className={`${styles.formItem} ${selectedForm === 'PAYMENT_DETAILS' ? styles.active : ''}`}
                onClick={() => handleFormSelect('PAYMENT_DETAILS')}
            >
                Reviewer Insights
                <FontAwesomeIcon icon={faChevronRight} className={`${styles.chevronIcon} ${selectedForm === 'PAYMENT_DETAILS' ? styles.active : ''}`} />
            </div>
            <div
                className={`${styles.formItem} ${selectedForm === 'LOST_POLICY_FORM' ? styles.active : ''}`}
                onClick={() => handleFormSelect('LOST_POLICY_FORM')}
            >
                Checklist
                <FontAwesomeIcon icon={faChevronRight} className={`${styles.chevronIcon} ${selectedForm === 'LOST_POLICY_FORM' ? styles.active : ''}`} />
            </div>
        </div>

            {renderFormData()}
        </div>
    );
};

export default NewFormDisplay;



/* Main container */
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
    gap: 40px;
    padding-top: 0;
}

/* Form selection section */
.formSelection {
    display: flex;
    gap: 15px;
    justify-content: center;
    flex-wrap: wrap;
    padding-bottom: 20px;
}

/* Individual form button */
.formItem {
    background-color: #1f2937;
    color: #f8fafc;
    padding: 12px 20px;
    padding-top: 6px;
    border-radius: 0px 0px 8px 8px;
    font-size: 0.8rem;
    font-weight: bold;
    text-transform: uppercase;
    cursor: pointer;
    transition: background 0.3s ease, transform 0.4s ease;
    display: flex;
    align-items: center;
    /*gap: 10px;*/
}

.formItem:hover, .formItem.active {
    background: linear-gradient(135deg, #f2f2f2 -20%, #7ca2e1);
    color: #1f2937;
}

.chevronIcon {
    color: #f8fafc;
    transition: transform 0.3s ease;
    margin-left: 10px;
}

.chevronIcon:hover{
        color: #000;
}

 .active{
        color: #1f2937;
 }

.cardIcon{
    margin-right: 10px;
}

.formItem:hover .chevronIcon {
    transform: translateX(5px);
}

/* Consistent layout for form data */
.formDataSection {
    background-color: #f1f5f9;
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
    width: 100%;
    max-width: 800px;
    display: flex;
    flex-direction: column;
    gap: 20px;
    color: #374151;
}

/* Titles for each section in form data */
.formDataTitle {
    font-size: 1rem;
    font-weight: 600;
    color: #2563eb;
}

/* Content in form data */
.formDataContent {
    font-size: .9rem;
    color: #1f2937;
    padding-left: 15px;
    line-height: 1.6;
}

/* Hover effect */
.formDataSection:hover {
    box-shadow: 0 12px 28px rgba(0, 0, 0, 0.2);
}

/* Loading and select messages */
.loadingMessage,
.selectMessage {
    font-size: 1.5rem;
    color: #e2e8f0;
    text-align: center;
    margin-top: 20px;
}

/* Responsive adjustments */
@media (max-width: 768px) {
    .formDisplay {
        padding: 15px;
    }

    .formDataTitle {
        font-size: 1rem;
    }
    
    .formDataContent {
        font-size: 0.9rem;
    }
}

