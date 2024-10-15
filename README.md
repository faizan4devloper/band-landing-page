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
                    <div key={key} className={styles.dataRow}>
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
        <div className={styles.dashboardContainer}>
            <div className={styles.card}>
                <h3>Verification Status</h3>
                {renderFormData('claimassist-history-lambda')}
            </div>

            <div className={styles.card}>
                <h3>Reviewer Insights</h3>
                {renderFormData('claimassist-docextract-lambda')}
            </div>

            <div className={styles.card}>
                <h3>Checklist</h3>
                {renderFormData('claimassist-checklist-lambda')}
            </div>
        </div>
    );
};

export default NewFormDisplay;




/* Main container for the whole dashboard */
.dashboardContainer {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-gap: 30px;
    padding: 40px;
    background-color: #f8fafc;
    max-width: 1200px;
    margin: 0 auto;
    height: 100%;
}

/* Styling each card to look more like widgets */
.card {
    background-color: #ffffff;
    border: 1px solid #e2e8f0;
    border-radius: 12px;
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
    padding: 20px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    transition: transform 0.3s, box-shadow 0.3s;
}

.card:hover {
    transform: translateY(-5px);
    box-shadow: 0 15px 25px rgba(0, 0, 0, 0.15);
}

/* Card Header */
.card h3 {
    font-size: 1.2rem;
    color: #1e293b;
    margin-bottom: 20px;
    border-bottom: 2px solid #3b82f6;
    padding-bottom: 10px;
}

/* Form Data Section */
.formDataSection {
    margin-top: 20px;
}

/* Form Data Title */
.formDataTitle {
    font-size: 1rem;
    font-weight: 600;
    color: #1d4ed8;
    display: flex;
    align-items: center;
}

/* Form Data Content */
.formDataContent {
    font-size: 0.85rem;
    color: #4b5563;
    margin-left: 30px;
    margin-top: 5px;
}

/* Icon styling */
.cardIcon {
    margin-right: 10px;
    font-size: 1rem;
    color: #6b7280;
}

/* Individual rows of data */
.dataRow {
    margin-bottom: 15px;
}

/* Responsive Layout */
@media (max-width: 1024px) {
    .dashboardContainer {
        grid-template-columns: repeat(2, 1fr);
    }
}

@media (max-width: 768px) {
    .dashboardContainer {
        grid-template-columns: 1fr;
    }
}
