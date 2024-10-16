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
            {['Verification Status', 'Reviewer Insights', 'Checklist'].map((section) => (
                <div className={`${styles.card} ${expanded[section] ? styles.expandedCard : ''}`} key={section}>
                    <div className={styles.cardHeader} onClick={() => toggleExpanded(section)}>
                        <h3>{section}</h3>
                        <FontAwesomeIcon
                            icon={faChevronRight}
                            className={`${styles.chevronIcon} ${expanded[section] ? styles.expanded : ''}`}
                        />
                    </div>
                    <div className={styles.cardContent}>
                        {renderFormData('claimassist-history-lambda', expanded[section])}
                    </div>
                    <button className={styles.viewAllButton} onClick={() => toggleExpanded(section)}>
                        {expanded[section] ? 'View Less' : 'View All'}
                    </button>
                </div>
            ))}
        </div>
    );
};

export default NewFormDisplay;


/* Main container for the whole dashboard */
.dashboardContainer {
    display: grid;
    grid-template-columns: 1fr;
    grid-gap: 20px;
    padding: 20px;
    background: linear-gradient(135deg, #1e293b, #334155);
    margin: 0 auto;
}

/* Styling each card (like a widget) */
.card {
    background-color: #fff;
    border: 1px solid #d1d5db;
    border-left: 5px solid #7ca2e1;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    padding: 15px;
    color: #374151;
    border-radius: 8px;
    transition: max-height 0.3s ease, box-shadow 0.3s ease;
    overflow: hidden;
    max-height: 100px; /* Collapsed state */
}

.expandedCard {
    max-height: 600px; /* Expanded state */
}

/* Sub-card container */
.subCardContainer {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
}

/* Each sub-card for displaying individual sections */
.subCard {
    background: linear-gradient(135deg, #8cbffd, #444f6c);
    border-radius: 8px;
    padding: 10px;
    color: white;
    flex: 1 1 calc(50% - 10px);
    transition: transform 0.3s ease, background 0.3s ease;
}

.subCardTitle {
    font-size: 1rem;
    font-weight: 600;
    display: flex;
    align-items: center;
    margin-bottom: 5px;
}

.subCardContent {
    font-size: 0.85rem;
}

.cardHeader {
    display: flex;
    justify-content: space-between;
    align-items: center;
    cursor: pointer;
    padding-bottom: 10px;
    border-bottom: 1px solid #d1d5db;
    margin-bottom: 10px;
}

.chevronIcon {
    transition: transform 0.3s ease;
}

.expanded .chevronIcon {
    transform: rotate(90deg);
}

.viewAllButton {
    background: none;
    font-size: 0.9rem;
    font-weight: 600;
    margin-top: 20px;
    border: none;
    cursor: pointer;
    color: #2563eb;
    transition: color 0.2s ease;
}

.viewAllButton:hover {
    color: #000;
}

/* Responsive Layout */
@media (max-width: 768px) {
    .dashboardContainer {
        grid-template-columns: 1fr;
    }
    .subCard {
        flex: 1 1 100%;
    }
}










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
            {/* Use unique keys for expanded state but same data source */}
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
    background: linear-gradient(135deg, #1e293b, #334155);
    margin: 0 auto;
}

/* Styling each card (like a widget) */
.card {
    background-color: #fff;
    border: 1px solid #d1d5db;
    border-left: 5px solid #7ca2e1;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    padding: 15px;
    color: #374151;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    transition: transform 0.2s ease, box-shadow 0.2s ease;
    border-radius: 8px;
}

.card:hover {
    transform: translateY(-5px);
    box-shadow: 0 6px 18px rgba(0, 0, 0, 0.15);
}

/* Sub-card container */
.subCardContainer {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
}

/* Each sub-card for displaying individual sections */
.subCard {
    /*background: linear-gradient(135deg, #F2F2F2 -20%, #7ca2e1);*/
    background: linear-gradient(135deg, #8cbffd, #444f6c);
    border-radius: 8px;
    padding: 10px;
    color: white;
    flex: 1 1 calc(50% - 10px);
    transition: transform 0.3s ease, background 0.3s ease;
}

/*.subCard:hover {*/
    /*background: linear-gradient(135deg, #60a5fa, #1d4ed8);*/
/*        background: linear-gradient(135deg, #F2F2F2 -20%, #7ca2e1);*/

/*}*/

/* Sub-card title */
.subCardTitle {
    font-size: 1rem;
    font-weight: 600;
    display: flex;
    align-items: center;
    margin-bottom: 5px;
}

/* Sub-card content */
.subCardContent {
    font-size: 0.85rem;
}

/* Icon styling */
.cardIcon {
    margin-right: 8px;
    color: white;
    font-size: 1.1rem;
}

/* Button styling */
.viewAllButton {
    background: none;
    font-size: 0.9rem;
    font-weight: 600;
    margin-top: 20px;
    border: none;
    cursor: pointer;
    color: #2563eb;
    transition: color 0.2s ease;
}

.viewAllButton:hover {
    color: #000;
}

/* Responsive Layout */
@media (max-width: 768px) {
    .dashboardContainer {
        grid-template-columns: 1fr;
    }
    .subCard {
        flex: 1 1 100%;
    }
}
