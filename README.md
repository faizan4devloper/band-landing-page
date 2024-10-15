import React, { useEffect, useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import {
    faChevronRight,
    faSignature,
    faIdCard,
    faFileAlt,
    faCalendarAlt,
    faQuestionCircle,
    faInfoCircle
} from '@fortawesome/free-solid-svg-icons';
import formDataJson from '../../../Json/NewEntities.json';
import styles from './NewFormDisplay.module.css';

const NewFormDisplay = () => {
    const [formData, setFormData] = useState(null);
    const [expanded, setExpanded] = useState({});

    useEffect(() => {
        setFormData(formDataJson);
    }, []);

    const toggleExpanded = (key) => {
        setExpanded((prev) => ({
            ...prev,
            [key]: !prev[key],
        }));
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

    const renderFormData = (formKey, isExpanded) => {
        if (!formData || !formData[formKey]) return null;

        const selectedData = formData[formKey];
        const entries = Object.entries(selectedData);

        // Show fewer entries when collapsed
        const displayedEntries = isExpanded ? entries : entries.slice(0, 3);

        return (
            <div className={styles.formDataSection}>
                {displayedEntries.map(([key, value]) => (
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
                {renderFormData('claimassist-history-lambda', expanded['Verification Status'])}
                <button className={styles.viewAllButton} onClick={() => toggleExpanded('Verification Status')}>
                    {expanded['Verification Status'] ? 'View Less' : 'View All'}
                </button>
            </div>

            <div className={styles.card}>
                <h3>Reviewer Insights</h3>
                {renderFormData('claimassist-history-lambda', expanded['Reviewer Insights'])}
                <button className={styles.viewAllButton} onClick={() => toggleExpanded('Reviewer Insights')}>
                    {expanded['Reviewer Insights'] ? 'View Less' : 'View All'}
                </button>
            </div>

            <div className={styles.card}>
                <h3>Checklist</h3>
                {renderFormData('claimassist-history-lambda', expanded['Checklist'])}
                <button className={styles.viewAllButton} onClick={() => toggleExpanded('Checklist')}>
                    {expanded['Checklist'] ? 'View Less' : 'View All'}
                </button>
            </div>
        </div>
    );
};

export default NewFormDisplay;

/* Main container for the whole dashboard */
.dashboardContainer {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-gap: 20px;
    height: 100%;
    padding: 20px;
    background:linear-gradient(135deg, #1e293b, #334155);
    margin: 0 auto;
}

/* Styling each card (similar to widgets in the screenshot) */
.card {
    background-color: #fff;
    border: 1px solid #d1d5db;
    border-radius: 10px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
    padding: 10px;
    color: #374151;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

/* Card Header */
.card h3 {
    font-size: 1rem;
    color: #1f2937;
    margin-bottom: 10px;
}

/* Form Data Section */
.formDataSection {
    margin-top: 20px;
}

.formDataTitle {
    font-size: .8rem;
    font-weight: 600;
    color: #2563eb;
}

.formDataContent {
    font-size: 0.7rem;
    color: #4b5563;
}

/* Icon styling */
.cardIcon {
    margin-right: 8px;
    color: #6b7280;
}

.viewAllButton{
    background:none;
    border:none;
    /*width: 50px;*/
}

/* Responsive Layout */
@media (max-width: 768px) {
    .dashboardContainer {
        grid-template-columns: 1fr;
    }
}
