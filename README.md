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
import formDataJson from '../../../Json/NewEntities.json'; // Make sure the path is correct
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

    const renderNestedObject = (obj) => {
        return (
            <div>
                {Object.entries(obj).map(([key, value]) => (
                    <div key={key} className={styles.nestedObject}>
                        <h4 className={styles.formDataTitle}>{key.replace(/_/g, ' ')}</h4>
                        {typeof value === 'object' ? renderNestedObject(value) : <p className={styles.formDataContent}>{value}</p>}
                    </div>
                ))}
            </div>
        );
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
                        {Array.isArray(value) ? (
                            <ul className={styles.verificationList}>
                                {value.map((item, index) => (
                                    <li key={index} className={styles.verificationItem}>
                                        <FontAwesomeIcon icon={faChevronRight} className={styles.listIcon} />
                                        {item}
                                    </li>
                                ))}
                            </ul>
                        ) : typeof value === 'object' ? (
                            renderNestedObject(value)
                        ) : (
                            <p className={styles.formDataContent}>{value}</p>
                        )}
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

/* Styling each card (similar to widgets in the screenshot) */
.card {
    background-color: #ffffff;
    border-radius: 10px;
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
    padding: 20px;
    color: #374151;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    transition: transform 0.3s, box-shadow 0.3s; /* Animation on hover */
}

.card:hover {
    transform: translateY(-5px); /* Lift effect on hover */
    box-shadow: 0 12px 30px rgba(0, 0, 0, 0.15); /* Enhanced shadow on hover */
}

/* Card Header */
.card h3 {
    font-size: 1.25rem; /* Increased font size */
    color: #1f2937;
    margin-bottom: 15px;
    border-bottom: 2px solid #2563eb; /* Bottom border for distinction */
    padding-bottom: 10px; /* Space between text and border */
}

/* Form Data Section */
.formDataSection {
    margin-top: 20px;
}

.formDataTitle {
    font-size: 1rem; /* Increased font size for titles */
    font-weight: 600;
    color: #2563eb;
    margin-bottom: 8px;
}

.formDataContent {
    font-size: 0.9rem; /* Adjust font size */
    color: #4b5563;
    line-height: 1.5; /* Better readability */
}

/* List styles for Verification Status */
.verificationList {
    list-style: none; /* Remove default bullet points */
    padding-left: 0; /* Remove padding */
}

.verificationItem {
    display: flex;
    align-items: center; /* Align icon and text vertically */
    margin-bottom: 12px; /* Space between items */
    font-size: 0.9rem; /* Font size adjustment */
    color: #374151; /* Text color for list items */
    transition: color 0.3s; /* Color transition */
}

.verificationItem:hover {
    color: #2563eb; /* Change color on hover */
}

.listIcon {
    margin-right: 8px; /* Space between icon and text */
    color: #2563eb; /* Icon color */
}

/* Nested object styling */
.nestedObject {
    margin-top: 10px; /* Space between nested sections */
}

.nestedObject h4 {
    font-size: 0.85rem; /* Adjusted font size for nested keys */
    font-weight: 500;
    color: #374151;
}

/* Responsive Layout */
@media (max-width: 768px) {
    .dashboardContainer {
        grid-template-columns: 1fr;
    }
}
