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

                {/* Additional Section 1 */}
                <div className={styles.cardSection}>
                    <h4>Recent Updates</h4>
                    <p>Review the latest changes and updates.</p>
                </div>

                {/* Additional Section 2 */}
                <div className={styles.cardSection}>
                    <h4>Performance Overview</h4>
                    <p>Details about overall performance metrics.</p>
                </div>

                <button className={styles.viewAllButton} onClick={() => toggleExpanded('Verification Status')}>
                    {expanded['Verification Status'] ? 'View Less' : 'View All'}
                </button>
            </div>

            <div className={styles.card}>
                <h3>Reviewer Insights</h3>
                {renderFormData('claimassist-history-lambda', expanded['Reviewer Insights'])}

                {/* Additional Section 1 */}
                <div className={styles.cardSection}>
                    <h4>Reviewer's Comments</h4>
                    <p>Comments and insights from the reviewer.</p>
                </div>

                {/* Additional Section 2 */}
                <div className={styles.cardSection}>
                    <h4>Next Steps</h4>
                    <p>Suggested actions for further review.</p>
                </div>

                <button className={styles.viewAllButton} onClick={() => toggleExpanded('Reviewer Insights')}>
                    {expanded['Reviewer Insights'] ? 'View Less' : 'View All'}
                </button>
            </div>

            <div className={styles.card}>
                <h3>Checklist</h3>
                {renderFormData('claimassist-history-lambda', expanded['Checklist'])}

                {/* Additional Section 1 */}
                <div className={styles.cardSection}>
                    <h4>Completed Tasks</h4>
                    <p>List of tasks that have been completed.</p>
                </div>

                {/* Additional Section 2 */}
                <div className={styles.cardSection}>
                    <h4>Pending Tasks</h4>
                    <p>List of tasks still pending review.</p>
                </div>

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
    padding: 20px;
    background: linear-gradient(135deg, #1e293b, #334155);
    width: 100%;
}

/* Styling each card */
.card {
    background-color: #fff;
    border: 1px solid #d1d5db;
    border-radius: 10px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
    padding: 20px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    transition: all 0.3s ease-in-out;
}

/* Hover effect for card */
.card:hover {
    transform: translateY(-5px);
    box-shadow: 0 6px 16px rgba(0, 0, 0, 0.15);
}

/* Card Header */
.card h3 {
    font-size: 1.2rem;
    color: #1f2937;
    margin-bottom: 15px;
    border-bottom: 2px solid #d1d5db;
    padding-bottom: 10px;
}

/* Form Data Section */
.formDataSection {
    margin-top: 20px;
}

.formDataTitle {
    font-size: 0.9rem;
    font-weight: 600;
    color: #2563eb;
    display: flex;
    align-items: center;
    margin-bottom: 5px;
}

.formDataContent {
    font-size: 0.8rem;
    color: #4b5563;
    margin-left: 25px;
}

/* Icon styling */
.cardIcon {
    margin-right: 10px;
    color: #6b7280;
}

/* Styling for additional sections inside the cards */
.cardSection {
    padding: 15px;
    background-color: #f3f4f6;
    border-radius: 8px;
    margin-top: 20px;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
}

.cardSection h4 {
    font-size: 1rem;
    color: #111827;
    margin-bottom: 10px;
}

.cardSection p {
    font-size: 0.85rem;
    color: #374151;
}

/* "View All" button */
.viewAllButton {
    background-color: #2563eb;
    color: #fff;
    border: none;
    padding: 8px 12px;
    font-size: 0.9rem;
    cursor: pointer;
    margin-top: 20px;
    border-radius: 5px;
    align-self: flex-end;
    transition: background-color 0.3s ease;
}

.viewAllButton:hover {
    background-color: #1d4ed8;
}

/* Responsive Layout */
@media (max-width: 768px) {
    .dashboardContainer {
        grid-template-columns: 1fr;
    }
}
