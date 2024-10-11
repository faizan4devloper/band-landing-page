import React, { useEffect, useState } from 'react';
import { useNavigate } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import {
    faChevronRight, faSignature, faIdCard, faFileAlt, faCalendarAlt,
    faQuestionCircle, faInfoCircle
} from '@fortawesome/free-solid-svg-icons';
import formDataJson from '../../../Json/NewEntities.json';
import styles from './NewFormDisplay.module.css';

const NewFormDisplay = ({ selectedForm }) => {
    const [formData, setFormData] = useState(null);
    const navigate = useNavigate();

    useEffect(() => {
        setFormData(formDataJson);
    }, []);

    const handleNextClick = () => {
        navigate('/New-page');
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
            <div className={styles.formContent}>
                <div className={styles.cardsWrapper}>
                    {Object.entries(selectedData).map(([key, value]) => (
                        <div key={key} className={styles.formCard}>
                            <div className={styles.formLabel}>
                                <FontAwesomeIcon icon={chooseIcon(key)} className={styles.formIcon} />
                                {key.replace(/_/g, ' ')}
                            </div>
                            <div className={styles.formValue}>
                                {Array.isArray(value) ? value.join(', ') : value}
                            </div>
                        </div>
                    ))}
                </div>
                <div className={styles.actionSection}>
                    <button className={styles.nextBtn} onClick={handleNextClick}>
                        Next <FontAwesomeIcon icon={faChevronRight} className={styles.nextIcon} />
                    </button>
                </div>
            </div>
        );
    };

    return (
        <div className={styles.container}>
            <div className={styles.descriptionSection}>
                <h2>Form Description</h2>
                <p>Details and metadata about the selected form.</p>
            </div>
            <div className={styles.formDisplaySection}>
                {renderFormData()}
            </div>
            <div className={styles.uploadSection}>
                <h3>Uploaded Files</h3>
                <div className={styles.uploadArea}>
                    {/* You can add file upload inputs here */}
                    <p>Drag and drop files or click to upload.</p>
                </div>
            </div>
            <div className={styles.historySection}>
                <h3>History</h3>
                <ul>
                    {/* Add history items here */}
                    <li>Previous form submission - 04 Feb, 2023</li>
                    <li>Another entry - 02 Mar, 2023</li>
                </ul>
            </div>
        </div>
    );
};

export default NewFormDisplay;

/* Main container layout */
.container {
    display: grid;
    grid-template-columns: 1fr 2fr 1fr;
    grid-gap: 2rem;
    padding: 2rem;
    background-color: #f4f5f7;
    font-family: 'Inter', sans-serif;
}

/* Description section */
.descriptionSection {
    background-color: #fff;
    padding: 1.5rem;
    border-radius: 12px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.descriptionSection h2 {
    font-size: 1.5rem;
    color: #1f2937;
    margin-bottom: 1rem;
}

.descriptionSection p {
    color: #6b7280;
    font-size: 1rem;
}

/* Form display section */
.formDisplaySection {
    background-color: #fff;
    padding: 2rem;
    border-radius: 12px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

.cardsWrapper {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 1rem;
}

.formCard {
    background-color: #eef2f7;
    padding: 1.2rem;
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
}

.formLabel {
    font-weight: bold;
    color: #2563eb;
    display: flex;
    align-items: center;
}

.formIcon {
    margin-right: 0.5rem;
    color: #60a5fa;
}

.formValue {
    margin-top: 0.5rem;
    color: #374151;
}

/* Action section */
.actionSection {
    margin-top: 2rem;
    text-align: right;
}

.nextBtn {
    background-color: #2563eb;
    color: #fff;
    padding: 10px 20px;
    border-radius: 8px;
    border: none;
    cursor: pointer;
    transition: all 0.3s ease;
}

.nextBtn:hover {
    background-color: #1e40af;
    transform: translateY(-2px);
}

.nextIcon {
    margin-left: 8px;
}

/* Upload section */
.uploadSection {
    background-color: #fff;
    padding: 1.5rem;
    border-radius: 12px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.uploadArea {
    background-color: #f9fafb;
    padding: 2rem;
    border: 2px dashed #d1d5db;
    border-radius: 8px;
    text-align: center;
    color: #6b7280;
}

/* History section */
.historySection {
    background-color: #fff;
    padding: 1.5rem;
    border-radius: 12px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.historySection h3 {
    margin-bottom: 1rem;
    color: #1f2937;
}

.historySection ul {
    list-style: none;
    padding: 0;
}

.historySection li {
    padding: 0.5rem 0;
    border-bottom: 1px solid #e5e7eb;
    color: #6b7280;
}
