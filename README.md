/* NewFormDisplay.module.css */

.formDisplay {
    padding: 20px;
    background-color: #f8f9fa; /* Light background for contrast */
    border-radius: 8px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.loadingMessage,
.selectMessage {
    text-align: center;
    font-size: 18px;
    color: #6c757d; /* Gray color for loading messages */
}

.gridContainer {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 20px;
}

.card {
    background-color: #ffffff; /* White background for cards */
    border-radius: 8px;
    padding: 15px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
    transition: transform 0.2s, box-shadow 0.2s;
}

.card:hover {
    transform: translateY(-5px); /* Lift effect on hover */
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
}

.cardHeader {
    display: flex;
    align-items: center;
    font-size: 18px;
    color: #343a40; /* Dark color for headings */
    font-weight: 600;
}

.cardIcon {
    margin-right: 10px;
    color: #007bff; /* Primary color for icons */
}

.cardContent {
    margin-top: 10px;
    font-size: 16px;
    color: #495057; /* Slightly lighter dark color for content */
}






import React, { useEffect, useState } from 'react';
import { useNavigate } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronRight, faSignature, faIdCard, faFileAlt, faCalendarAlt, faQuestionCircle, faInfoCircle } from '@fortawesome/free-solid-svg-icons';
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
            <h2>{selectedForm.replace(/-/g, ' ').toUpperCase()}</h2> {/* Display the selected form name */}
            {renderFormData()}
            <button className={styles.nextButton} onClick={handleNextClick}>
                Next <FontAwesomeIcon icon={faChevronRight} />
            </button>
        </div>
    );
};

export default NewFormDisplay;
