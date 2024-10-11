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
                <button className={styles.nextBtn} onClick={handleNextClick}>
                    Next <FontAwesomeIcon icon={faChevronRight} className={styles.nextIcon} />
                </button>

                <div className={styles.formTable}>
                    {Object.entries(selectedData).map(([key, value]) => (
                        <div key={key} className={styles.formRow}>
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
            </div>
        );
    };

    return (
        <div className={styles.container}>
            <div className={styles.sidebar}>
                <h2>Forms</h2>
                {/* Add other sidebar elements here */}
            </div>
            <div className={styles.mainContent}>
                {renderFormData()}
            </div>
        </div>
    );
};

export default NewFormDisplay;


/* Main container */
.container {
    display: flex;
    height: 100vh;
    background-color: #f8f9fa;
    font-family: 'Inter', sans-serif;
}

/* Sidebar */
.sidebar {
    width: 240px;
    background-color: #fff;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    padding: 20px;
}

.sidebar h2 {
    font-size: 1.5rem;
    color: #1f2937;
    margin-bottom: 1rem;
}

/* Main content area */
.mainContent {
    flex-grow: 1;
    padding: 40px;
    overflow-y: auto;
    background-color: #f1f5f9;
}

.formContent {
    background-color: #fff;
    padding: 30px;
    border-radius: 10px;
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
}

/* Table styling */
.formTable {
    display: grid;
    grid-template-columns: 1fr 2fr;
    gap: 1rem;
}

.formRow {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 10px 0;
    border-bottom: 1px solid #e5e7eb;
}

.formLabel {
    display: flex;
    align-items: center;
    color: #2563eb;
    font-weight: bold;
}

.formIcon {
    margin-right: 10px;
    color: #7ca2e1;
}

.formValue {
    color: #374151;
    font-size: 0.9rem;
}

/* Next button */
.nextBtn {
    background-color: #1e40af;
    color: #fff;
    padding: 12px 24px;
    border: none;
    border-radius: 6px;
    cursor: pointer;
    margin-top: 20px;
    font-size: 1rem;
    transition: background-color 0.3s ease, transform 0.2s ease;
}

.nextBtn:hover {
    background-color: #1d4ed8;
    transform: scale(1.05);
}

.nextIcon {
    margin-left: 8px;
    transition: transform 0.3s ease;
}

.nextBtn:hover .nextIcon {
    transform: translateX(5px);
}

/* Responsive design */
@media (max-width: 768px) {
    .formTable {
        grid-template-columns: 1fr;
    }

    .sidebar {
        width: 180px;
    }

    .mainContent {
        padding: 20px;
    }
}
