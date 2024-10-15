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



/* Additional styles for the Verification Status list */
.verificationList {
    list-style: none; /* Remove default bullet points */
    padding-left: 0; /* Remove padding */
}

.verificationItem {
    display: flex;
    align-items: center; /* Align icon and text vertically */
    margin-bottom: 8px; /* Space between items */
    font-size: 0.8rem; /* Font size adjustment */
    color: #4b5563; /* Text color for list items */
}

.listIcon {
    margin-right: 8px; /* Space between icon and text */
}

/* Additional styles for Reviewer Insights structured data */
.nestedObject {
    margin-top: 10px; /* Space between nested sections */
}

/* Style the card header */
.card h3 {
    font-size: 1rem;
    color: #1f2937;
    margin-bottom: 10px;
    border-bottom: 2px solid #2563eb; /* Add a bottom border for distinction */
    padding-bottom: 5px; /* Space between text and border */
}

/* Responsive Layout */
@media (max-width: 768px) {
    .dashboardContainer {
        grid-template-columns: 1fr;
    }
}
