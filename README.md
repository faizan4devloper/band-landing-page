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

    const renderFormData = (formKey) => {
        if (!formData || !formData[formKey]) return null;

        const selectedData = formData[formKey];

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
            <div className={styles.formColumns}>
                {/* Verification Status */}
                <div className={styles.formColumn}>
                    <h3>Verification Status</h3>
                    {renderFormData('claimassist-history-lambda')}
                </div>

                {/* Reviewer Insights */}
                <div className={styles.formColumn}>
                    <h3>Reviewer Insights</h3>
                    {renderFormData('PAYMENT_DETAILS')}
                </div>

                {/* Checklist */}
                <div className={styles.formColumn}>
                    <h3>Checklist</h3>
                    {renderFormData('LOST_POLICY_FORM')}
                </div>
            </div>
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
    font-family: 'Poppins', sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
}

/* Side-by-side layout */
.formColumns {
    display: flex;
    gap: 20px;
    width: 100%;
    max-width: 1200px;
    justify-content: space-between;
}

.formColumn {
    background-color: #f1f5f9;
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 20px;
    color: #374151;
}

.formColumn h3 {
    font-size: 1.2rem;
    font-weight: 600;
    color: #2563eb;
    margin-bottom: 15px;
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
.formColumn:hover {
    box-shadow: 0 12px 28px rgba(0, 0, 0, 0.2);
}

/* Card icon styling */
.cardIcon {
    margin-right: 10px;
}

/* Responsive adjustments */
@media (max-width: 768px) {
    .formColumns {
        flex-direction: column;
        gap: 30px;
    }

    .formColumn {
        width: 100%;
    }
}
