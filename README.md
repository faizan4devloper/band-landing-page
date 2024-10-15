import React, { useEffect, useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import {
    faSignature,
    faIdCard,
    faFileAlt,
    faCalendarAlt,
    faQuestionCircle,
    faInfoCircle
} from '@fortawesome/free-solid-svg-icons';
import formDataJson from '../../../Json/NewEntities.json'; // Make sure this path is correct
import styles from './NewFormDisplay.module.css';

const NewFormDisplay = () => {
    const [formData, setFormData] = useState(null);
    const [expanded, setExpanded] = useState({
        verificationStatus: false,
        reviewerInsights: false,
        checklist: false
    });

    useEffect(() => {
        setFormData(formDataJson);
    }, []);

    // Toggle expanded state for a specific section
    const toggleExpanded = (section) => {
        setExpanded((prev) => ({
            ...prev,
            [section]: !prev[section],
        }));
    };

    // Chooses the correct icon based on the section name
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

    // Renders data for a specific section
    const renderFormData = (formKey, isExpanded) => {
        if (!formData || !formData[formKey]) return null;

        const selectedData = formData[formKey];
        const entries = Object.entries(selectedData);

        // Show first 3 entries if collapsed
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
            {/* Section 1: Verification Status */}
            <div className={styles.card}>
                <h3>Verification Status</h3>
                {renderFormData('verification_status', expanded.verificationStatus)} 
                <button className={styles.viewAllButton} onClick={() => toggleExpanded('verificationStatus')}>
                    {expanded.verificationStatus ? 'View Less' : 'View All'}
                </button>
            </div>

            {/* Section 2: Reviewer Insights */}
            <div className={styles.card}>
                <h3>Reviewer Insights</h3>
                {renderFormData('reviewer_insights', expanded.reviewerInsights)} 
                <button className={styles.viewAllButton} onClick={() => toggleExpanded('reviewerInsights')}>
                    {expanded.reviewerInsights ? 'View Less' : 'View All'}
                </button>
            </div>

            {/* Section 3: Checklist */}
            <div className={styles.card}>
                <h3>Checklist</h3>
                {renderFormData('checklist', expanded.checklist)} 
                <button className={styles.viewAllButton} onClick={() => toggleExpanded('checklist')}>
                    {expanded.checklist ? 'View Less' : 'View All'}
                </button>
            </div>
        </div>
    );
};

export default NewFormDisplay;
