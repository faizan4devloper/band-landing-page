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

    const toggleExpanded = (sectionKey) => {
        setExpanded((prev) => ({
            ...prev,
            [sectionKey]: !prev[sectionKey],
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
            <div className={styles.subCardContainer}>
                {displayedEntries.map(([key, value]) => (
                    <div key={key} className={styles.subCard}>
                        <h2 className={styles.subCardTitle}>
                            <FontAwesomeIcon icon={chooseIcon(key)} className={styles.cardIcon} />
                            {key.replace(/_/g, ' ')}
                        </h2>
                        <p className={styles.subCardContent}>
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
