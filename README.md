import React, { useEffect, useState } from 'react';
import { useNavigate } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowRight } from '@fortawesome/free-solid-svg-icons';
import formDataJson from '../../../Json/DocumentEntities.json';
import styles from './FormDisplay.module.css';

const FormDisplay = ({ selectedForm }) => {
    const [formData, setFormData] = useState(null);
    const navigate = useNavigate();

    useEffect(() => {
        setFormData(formDataJson.extracted_data);
    }, []);

    const handleNextClick = () => {
        navigate('/New-page');
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
            <div className={styles.cardContainer}>
                <h2 className={styles.formHead}>{selectedForm.replace(/_/g, ' ')}</h2>
                <button className={styles.nextBtn} onClick={handleNextClick}>
                    <FontAwesomeIcon icon={faArrowRight} className={styles.nextIcon} />
                </button>
                <div className={styles.cardGrid}>
                    {Object.entries(selectedData).map(([key, value]) => (
                        <div key={key} className={styles.card}>
                            <div className={styles.cardContent}>
                                <h3 className={styles.cardTitle}>{key.replace(/_/g, ' ')}</h3>
                                <p className={styles.cardText}>{value}</p>
                            </div>
                        </div>
                    ))}
                </div>
            </div>
        );
    };

    return (
        <div className={styles.formDisplay}>
            {renderFormData()}
        </div>
    );
};

export default FormDisplay;




/* Main container */
.formDisplay {
    background: rgba(0, 0, 0, 0.6);
    height: 100%;
    width: 100%;
    overflow-y: auto;
    gap: 20px;
    font-family: 'Arial', sans-serif;
}

/* Loading and select messages */
.loadingMessage,
.selectMessage {
    font-size: 1.5rem;
    color: #e2e8f0;
    text-align: center;
    margin-top: 20px;
}

/* Form heading */
.formHead {
    font-size: 1.5rem;
    font-weight: bold;
    color: #fff;
    text-align: left;
    margin-bottom: 25px;
    text-transform: capitalize;
}

/* Button */
.nextBtn {
    position: absolute;
    top: -40px;
    right: 40px;
    align-items: center;
    padding: 8px 16px;
    font-size: 1.2rem;
    color: #fff;
    background: rgba(0, 0, 0, 0.6);
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background 0.3s ease, box-shadow 0.3s ease;
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3);
}

.nextBtn:hover {
    background: linear-gradient(135deg, #F2F2F2 -20%, #7ca2e1);
    transform: scale(1.05);
}

/* Card grid layout */
.cardContainer {
    width: 100%;
    background-color: #f1f5f9;
    border-left: 4px solid #7ca2e1;
    padding: 20px;
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
}

.cardGrid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 20px;
}

/* Card styles */
.card {
    background-color: #ffffff;
    border-radius: 12px;
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
    padding: 20px;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.card:hover {
    transform: translateY(-10px);
    box-shadow: 0 16px 32px rgba(0, 0, 0, 0.15);
}

/* Card content */
.cardContent {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
}

.cardTitle {
    font-size: 1.2rem;
    font-weight: bold;
    color: #1f2937;
    margin-bottom: 10px;
    text-transform: capitalize;
}

.cardText {
    font-size: 1rem;
    color: #374151;
}

/* Mobile responsiveness */
@media (max-width: 768px) {
    .formHead {
        font-size: 1.8rem;
    }

    .nextBtn {
        font-size: 1rem;
    }
}
