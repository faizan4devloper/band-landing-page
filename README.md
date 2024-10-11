import React, { useEffect, useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronRight, faSignature, faIdCard, faFileAlt, faCalendarAlt, faQuestionCircle, faInfoCircle } from '@fortawesome/free-solid-svg-icons';
import formDataJson from '../../../Json/NewEntities.json';
import styles from './NewFormDisplay.module.css';

const NewFormDisplay = () => {
    const [formData, setFormData] = useState(null);

    useEffect(() => {
        setFormData(formDataJson);
    }, []);

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

        return (
            <div className={styles.formDataContainer}>
                {Object.entries(formData).map(([formName, formDetails]) => (
                    <div key={formName} className={styles.formDataSection}>
                        <h2 className={styles.formTitle}>{formName.replace(/_/g, ' ')}</h2>
                        {Object.entries(formDetails).map(([key, value]) => (
                            <div key={key}>
                                <h3 className={styles.formDataTitle}>
                                    <FontAwesomeIcon icon={chooseIcon(key)} className={styles.cardIcon} />
                                    {key.replace(/_/g, ' ')}
                                </h3>
                                <p className={styles.formDataContent}>
                                    {Array.isArray(value) ? value.join(', ') : value}
                                </p>
                            </div>
                        ))}
                    </div>
                ))}
            </div>
        );
    };

    return (
        <div className={styles.formDisplay}>
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
}

/* Container for all forms' data */
.formDataContainer {
    display: flex;
    flex-direction: column;
    gap: 30px;
}

/* Titles for form sections */
.formTitle {
    font-size: 1.5rem;
    color: #f2f2f2;
    margin-bottom: 20px;
}

/* Consistent layout for each form data section */
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
    font-size: 1.2rem;
    font-weight: 600;
    color: #2563eb;
}

/* Content in form data */
.formDataContent {
    font-size: 1rem;
    color: #1f2937;
    padding-left: 15px;
    line-height: 1.6;
}

/* Hover effect */
.formDataSection:hover {
    box-shadow: 0 12px 28px rgba(0, 0, 0, 0.2);
}

/* Loading and select messages */
.loadingMessage {
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
