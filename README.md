import React, { useEffect, useState } from 'react';
import { useNavigate } from 'react-router-dom'; 
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronRight } from '@fortawesome/free-solid-svg-icons';
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

    const renderList = (items) => (
        <ul className={styles.list}>
            {items.map((item, index) => (
                <li key={index} className={styles.listItem}>
                    {item}
                </li>
            ))}
        </ul>
    );

    const renderFormData = () => {
        if (!formData) {
            return <p className={styles.loadingMessage}>Loading data...</p>;
        }

        const selectedData = formData[selectedForm];

        if (!selectedData) {
            return <p className={styles.selectMessage}>Please select a form to view its data.</p>;
        }

        return (
            <div className={styles.container}>
                <h2 className={styles.formHead}>{selectedForm.replace(/_/g, ' ')}</h2>

                <button className={styles.nextBtn} onClick={handleNextClick}>
                    Next <FontAwesomeIcon icon={faChevronRight} className={styles.nextIcon} />
                </button>

                <div className={styles.dataSection}>
                    {Object.entries(selectedData).map(([key, value]) => (
                        <div key={key} className={styles.dataBlock}>
                            <h3 className={styles.dataTitle}>{key.replace(/_/g, ' ')}</h3>
                            {Array.isArray(value) ? renderList(value) : <p className={styles.dataText}>{value}</p>}
                        </div>
                    ))}
                </div>
            </div>
        );
    };

    return <div className={styles.formDisplay}>{renderFormData()}</div>;
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
    margin-bottom: 20px;
}

/* Data section */
.dataSection {
    display: flex;
    flex-direction: column;
    gap: 20px;
}

/* Data block for each key */
.dataBlock {
    background-color: #f1f5f9;
    border-left: 4px solid #7ca2e1;
    padding: 20px;
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
    transition: background 0.3s ease;
}

/* Hover effect for data blocks */
.dataBlock:hover {
    background-color: #e2e8f0;
}

/* Data title */
.dataTitle {
    font-size: 1.2rem;
    font-weight: bold;
    color: #1f2937;
    margin-bottom: 10px;
}

/* Data text (for non-list data) */
.dataText {
    font-size: 1rem;
    color: #374151;
}

/* List styling */
.list {
    padding-left: 20px;
    margin: 0;
    list-style-type: disc;
}

.listItem {
    font-size: 1rem;
    color: #374151;
    margin-bottom: 5px;
}

/* Next Button */
.nextBtn {
    display: inline-flex;
    align-items: center;
    padding: 0.60rem 2rem;
    font-size: 1.1rem;
    color: #fff;
    background: rgba(0, 0, 0, 0.6);
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background 0.3s ease, box-shadow 0.3s ease;
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3);
    overflow: hidden;
}

.nextBtn:hover {
    background: linear-gradient(135deg, #F2F2F2, #7ca2e1);
    transform: scale(1.05);
}

.nextIcon {
    margin-left: 10px;
    transition: transform 0.3s ease;
}

.nextBtn:hover .nextIcon {
    transform: translateX(5px);
}

/* Responsive Design */
@media (max-width: 768px) {
    .formHead {
        font-size: 1.2rem;
    }

    .listItem {
        font-size: 0.9rem;
    }

    .dataText {
        font-size: 0.9rem;
    }
}
