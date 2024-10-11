import React, { useEffect, useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronRight, faSignature, faIdCard, faFileAlt, faCalendarAlt, faQuestionCircle, faInfoCircle } from '@fortawesome/free-solid-svg-icons';
import formDataJson from '../../../Json/NewEntities.json';
import styles from './NewFormDisplay.module.css';

const NewFormDisplay = () => {
    const [formData, setFormData] = useState(null);
    const [selectedForm, setSelectedForm] = useState(''); // Manage the selected form state

    useEffect(() => {
        setFormData(formDataJson); // Set the entire JSON structure for now
    }, []);

    const handleFormSelect = (formName) => {
        setSelectedForm(formName); // Set the selected form when clicked
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

    const renderFormData = () => {
        if (!formData) {
            return <p className={styles.loadingMessage}>Loading data...</p>;
        }

        const selectedData = formData[selectedForm];

        if (!selectedData) {
            return <p className={styles.selectMessage}>Please select a form to view its data.</p>;
        }

        return (
            <div className={styles.gridContainer}>
                {Object.entries(selectedData).map(([key, value]) => (
                    <div key={key} className={styles.card}>
                        <div className={styles.cardHeader}>
                            <FontAwesomeIcon icon={chooseIcon(key)} className={styles.cardIcon} />
                            {key.replace(/_/g, ' ')}
                        </div>
                        <div className={styles.cardContent}>
                            {Array.isArray(value) ? value.join(', ') : value}
                        </div>
                    </div>
                ))}
            </div>
        );
    };

    return (
        <div className={styles.formDisplay}>
            {/* Form Selection Buttons */}
            <div className={styles.formSelection}>
                <div
                    className={`${styles.formItem} ${selectedForm === 'claimassist-history-lambda' ? styles.active : ''}`}
                    onClick={() => handleFormSelect('claimassist-history-lambda')}
                >
                    Verification Status
                    <FontAwesomeIcon icon={faChevronRight} className={styles.chevronIcon} />
                </div>
                <div
                    className={`${styles.formItem} ${selectedForm === 'PAYMENT_DETAILS' ? styles.active : ''}`}
                    onClick={() => handleFormSelect('PAYMENT_DETAILS')}
                >
                    Reviewer Insights
                    <FontAwesomeIcon icon={faChevronRight} className={styles.chevronIcon} />
                </div>
                <div
                    className={`${styles.formItem} ${selectedForm === 'LOST_POLICY_FORM' ? styles.active : ''}`}
                    onClick={() => handleFormSelect('LOST_POLICY_FORM')}
                >
                    Checklist
                    <FontAwesomeIcon icon={faChevronRight} className={styles.chevronIcon} />
                </div>
            </div>

            {/* Form Data Display */}
            {renderFormData()}
        </div>
    );
};

export default NewFormDisplay;







/* Main container */
.formDisplay {
    background: linear-gradient(135deg, #1e293b, #334155);
    padding: 30px;
    height: 100%;
    width: 100%;
    overflow-y: auto;
    font-family: 'Arial', sans-serif;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 30px;
}

/* Form selection section */
.formSelection {
    display: flex;
    gap: 15px;
    justify-content: center;
    flex-wrap: wrap;
    padding-bottom: 20px;
}

/* Individual form button */
.formItem {
    background-color: #1f2937;
    color: #f8fafc;
    padding: 15px 30px;
    border-radius: 8px;
    font-size: 1.1rem;
    font-weight: bold;
    text-transform: uppercase;
    cursor: pointer;
    transition: background 0.3s ease, transform 0.3s ease;
    display: flex;
    align-items: center;
    gap: 10px;
}

.formItem:hover {
    background-color: #2563eb;
    transform: translateY(-5px);
}

.formItem.active {
    background-color: #3b82f6;
}

/* Chevron icon in form item */
.chevronIcon {
    color: #f8fafc;
    transition: transform 0.3s ease;
}

.formItem:hover .chevronIcon {
    transform: translateX(5px);
}

/* Grid layout for cards */
.gridContainer {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 20px;
    width: 100%;
}

/* Card styles */
.card {
    background-color: #f1f5f9;
    border-radius: 10px;
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

.card:hover {
    transform: scale(1.05);
    box-shadow: 0 12px 28px rgba(0, 0, 0, 0.2);
}

/* Card header */
.cardHeader {
    background-color: #2563eb;
    color: white;
    padding: 15px;
    font-size: 1.2rem;
    font-weight: bold;
    text-transform: capitalize;
    display: flex;
    align-items: center;
    gap: 10px;
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
}

.cardIcon {
    color: #f8fafc;
}

/* Card content */
.cardContent {
    padding: 20px;
    font-size: 1rem;
    color: #374151;
}

/* Next button */
.nextBtn {
    margin-top: 20px;
    padding: 0.75rem 2rem;
    font-size: 1.1rem;
    color: #fff;
    background-color: #2563eb;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background 0.3s ease, box-shadow 0.3s ease;
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3);
    justify-self: center;
}

.nextBtn:hover {
    background-color: #3b82f6;
    transform: scale(1.05);
}

.nextIcon {
    margin-left: 10px;
    transition: transform 0.3s ease;
}

.nextBtn:hover .nextIcon {
    transform: translateX(5px);
}

/* Loading and select messages */
.loadingMessage,
.selectMessage {
    font-size: 1.5rem;
    color: #e2e8f0;
    text-align: center;
    margin-top: 20px;
}

/* Responsive design */
@media (max-width: 768px) {
    .formDisplay {
        padding: 15px;
    }

    .cardHeader {
        font-size: 1rem;
    }

    .nextBtn {
        font-size: 1rem;
        padding: 0.5rem 1.5rem;
    }
}
