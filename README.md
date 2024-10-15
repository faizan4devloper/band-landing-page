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
                <h3 className={styles.cardHeader}>Verification Status</h3>
                {renderFormData('claimassist-history-lambda', expanded['Verification Status'])}
                <button className={styles.viewAllButton} onClick={() => toggleExpanded('Verification Status')}>
                    {expanded['Verification Status'] ? 'View Less' : 'View All'}
                </button>
            </div>

            <div className={styles.card}>
                <h3 className={styles.cardHeader}>Reviewer Insights</h3>
                {renderFormData('claimassist-history-lambda', expanded['Reviewer Insights'])}
                <button className={styles.viewAllButton} onClick={() => toggleExpanded('Reviewer Insights')}>
                    {expanded['Reviewer Insights'] ? 'View Less' : 'View All'}
                </button>
            </div>

            <div className={styles.card}>
                <h3 className={styles.cardHeader}>Checklist</h3>
                {renderFormData('claimassist-history-lambda', expanded['Checklist'])}
                <button className={styles.viewAllButton} onClick={() => toggleExpanded('Checklist')}>
                    {expanded['Checklist'] ? 'View Less' : 'View All'}
                </button>
            </div>

            {/* New Sections */}
            <div className={styles.card}>
                <h3 className={styles.cardHeader}>Progress Overview</h3>
                {renderFormData('claimassist-progress-overview', expanded['Progress Overview'])}
                <button className={styles.viewAllButton} onClick={() => toggleExpanded('Progress Overview')}>
                    {expanded['Progress Overview'] ? 'View Less' : 'View All'}
                </button>
            </div>

            <div className={styles.card}>
                <h3 className={styles.cardHeader}>User Activity</h3>
                {renderFormData('claimassist-user-activity', expanded['User Activity'])}
                <button className={styles.viewAllButton} onClick={() => toggleExpanded('User Activity')}>
                    {expanded['User Activity'] ? 'View Less' : 'View All'}
                </button>
            </div>
        </div>
    );
};

export default NewFormDisplay;





/* Main container for the dashboard */
.dashboardContainer {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-gap: 20px;
    padding: 20px;
    background: linear-gradient(135deg, #1e293b, #334155);
    margin: 0 auto;
    height: 100%;
}

/* Styling each card */
.card {
    background-color: #fff;
    border: 1px solid #d1d5db;
    border-radius: 12px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    padding: 15px;
    color: #374151;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.card:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.15);
}

/* Card Header */
.cardHeader {
    font-size: 1.2rem;
    font-weight: 600;
    color: #2563eb;
    margin-bottom: 15px;
}

/* Form Data Section */
.formDataSection {
    margin-top: 10px;
}

.formDataTitle {
    font-size: 0.85rem;
    font-weight: 600;
    color: #2563eb;
}

.formDataContent {
    font-size: 0.75rem;
    color: #4b5563;
}

/* Icon styling */
.cardIcon {
    margin-right: 10px;
    color: #6b7280;
    font-size: 1rem;
}

/* View All Button */
.viewAllButton {
    background-color: #2563eb;
    border: none;
    color: white;
    font-size: 0.85rem;
    border-radius: 5px;
    padding: 6px 12px;
    cursor: pointer;
    margin-top: 10px;
    transition: background-color 0.3s ease;
}

.viewAllButton:hover {
    background-color: #1e40af;
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
