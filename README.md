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

        const displayedEntries = isExpanded ? entries : entries.slice(0, 2);

        return (
            <div className={styles.subCardContainer}>
                {displayedEntries.map(([key, value]) => (
                    <div key={key} className={styles.subCard}>
                        <FontAwesomeIcon icon={chooseIcon(key)} className={styles.cardIcon} />
                        <div>
                            <h2 className={styles.subCardTitle}>{key.replace(/_/g, ' ')}</h2>
                            <p className={styles.subCardContent}>
                                {Array.isArray(value) ? value.join(', ') : value}
                            </p>
                        </div>
                    </div>
                ))}
            </div>
        );
    };

    return (
        <div className={styles.dashboardContainer}>
            {['Verification Status', 'Reviewer Insights', 'Checklist'].map((section) => (
                <div className={styles.card} key={section}>
                    <h3>{section}</h3>
                    {renderFormData('claimassist-history-lambda', expanded[section])}
                    <button className={styles.viewAllButton} onClick={() => toggleExpanded(section)}>
                        {expanded[section] ? 'View Less' : 'View All'}
                    </button>
                </div>
            ))}
        </div>
    );
};

export default NewFormDisplay;




/* Container */
.dashboardContainer {
    display: grid;
    grid-template-columns: 1fr;
    gap: 15px;
    padding: 20px;
    background: linear-gradient(135deg, #f2f2f2, #e5e7eb);
    max-width: 900px;
    margin: 0 auto;
}

/* Card styling */
.card {
    background-color: #fff;
    border: 1px solid #e5e7eb;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    padding: 20px;
    border-radius: 10px;
}

/* Sub-card styling */
.subCardContainer {
    display: flex;
    flex-direction: column;
    gap: 10px;
}

.subCard {
    display: flex;
    align-items: flex-start;
    background: #f1f5f9;
    border-radius: 8px;
    padding: 10px;
}

.subCardTitle {
    font-size: 1rem;
    font-weight: 600;
}

.subCardContent {
    font-size: 0.875rem;
    margin-top: 5px;
}

/* Icon styling */
.cardIcon {
    margin-right: 10px;
    color: #3b82f6;
    font-size: 1.25rem;
}

/* Button styling */
.viewAllButton {
    background-color: transparent;
    border: none;
    color: #3b82f6;
    font-size: 0.875rem;
    cursor: pointer;
    margin-top: 10px;
}

.viewAllButton:hover {
    text-decoration: underline;
}

/* Responsive Layout */
@media (min-width: 768px) {
    .dashboardContainer {
        grid-template-columns: repeat(2, 1fr);
    }
}



