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
            <div className={styles.dataContainer}>
                <h2 className={styles.formHead}>{selectedForm.replace(/_/g, ' ')}</h2>
                <button className={styles.nextBtn} onClick={handleNextClick}>
                    <FontAwesomeIcon icon={faArrowRight} className={styles.nextIcon} />
                </button>
                <ul className={styles.dataList}>
                    {Object.entries(selectedData).map(([key, value]) => (
                        <li key={key} className={styles.dataItem}>
                            <span className={styles.dataKey}>{key.replace(/_/g, ' ')}:</span>
                            <span className={styles.dataValue}>{value}</span>
                        </li>
                    ))}
                </ul>
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

/* Data container */
.dataContainer {
    background-color: #f1f5f9;
    padding: 20px;
    border-left: 4px solid #7ca2e1;
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
}

/* List of data */
.dataList {
    list-style-type: none;
    padding: 0;
    margin: 0;
}

.dataItem {
    display: flex;
    justify-content: space-between;
    padding: 12px 0;
    border-bottom: 1px solid #e2e8f0;
}

.dataKey {
    font-weight: bold;
    color: #1f2937;
    text-transform: capitalize;
}

.dataValue {
    color: #374151;
    text-align: right;
    flex: 1;
    padding-left: 20px;
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
