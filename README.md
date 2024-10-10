import React, { useEffect, useState } from 'react';
import { useNavigate } from 'react-router-dom'; // Import useNavigate for navigation
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronRight, faSignature, faIdCard, faFileAlt, faCalendarAlt, faQuestionCircle, faInfoCircle } from '@fortawesome/free-solid-svg-icons';
import formDataJson from '../../../Json/NewEntities.json'; // Import the JSON file
import styles from './NewFormDisplay.module.css';

const NewFormDisplay = ({ selectedForm }) => {
    const [formData, setFormData] = useState(null);
    const navigate = useNavigate(); // Create navigate function

    useEffect(() => {
        // Set formData from the correct key within the JSON structure
        setFormData(formDataJson); // Set the entire JSON structure for now
    }, []);

    // Function to handle "Next" button click
    const handleNextClick = () => {
        // Navigate to the Verification page
        navigate('/New-page');
    };

    // Function to choose FontAwesome icons based on the form key
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

    // Render form data based on selected form in a table format
    const renderFormData = () => {
        if (!formData) {
            return <p className={styles.loadingMessage}>Loading data...</p>;
        }

        // Access correct part of JSON using selectedForm
        const selectedData = formData[selectedForm];

        if (!selectedData) {
            return <p className={styles.selectMessage}>Please select a form to view its data.</p>;
        }

        return (
            <div className={styles.tableContainer}>
                <h2 className={styles.formHead}>
                    <FontAwesomeIcon icon={faFileAlt} className={styles.headerIcon} />
                    {selectedForm.replace(/_/g, ' ')}
                </h2>

                <button className={styles.nextBtn} onClick={handleNextClick}> {/* Add onClick */}
                    Next <FontAwesomeIcon icon={faChevronRight} className={styles.nextIcon} />
                </button>

                <table className={styles.dataTable}>
                    <thead>
                        <tr>
                            <th className={styles.tableHeader}>Field</th>
                            <th className={styles.tableHeader}>Value</th>
                        </tr>
                    </thead>
                    <tbody>
                        {Object.entries(selectedData).map(([key, value]) => (
                            <tr key={key} className={styles.tableRow}>
                                <td className={styles.tableCell}>
                                    <FontAwesomeIcon icon={chooseIcon(key)} className={styles.tableIcon} />
                                    {key.replace(/_/g, ' ')}
                                </td>
                                <td className={styles.tableCell}>
                                    {Array.isArray(value) ? value.join(', ') : value}
                                </td>
                            </tr>
                        ))}
                    </tbody>
                </table>
            </div>
        );
    };

    return (
        <div className={styles.formDisplay}>
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
    display: flex;
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
    font-size: 1.8rem;
    font-weight: bold;
    color: #f8fafc;
    text-align: left;
    margin-bottom: 25px;
    text-transform: capitalize;
    display: flex;
    align-items: center;
}

.headerIcon {
    margin-right: 10px;
    color: #7ca2e1;
}

/* Table container */
.tableContainer {
    margin-top: 20px;
    width: 100%;
    background-color: #f1f5f9;
    border-left: 4px solid #7ca2e1;
    padding: 20px;
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
    overflow-x: auto;
}

/* Table styles */
.dataTable {
    width: 100%;
    border-collapse: separate;
    border-spacing: 0;
    background-color: #ffffff;
}

/* Table headers */
.tableHeader {
    font-size: 1rem;
    font-weight: bold;
    color: #1f2937;
    background-color: #f9fafb;
    padding: 16px 12px;
    text-align: left;
    border-bottom: 2px solid #e5e7eb;
    text-transform: capitalize;
}

/* Table rows */
.tableRow {
    transition: background-color 0.3s ease;
}

.tableRow:hover {
    background-color: #e2e8f0;
}

/* Table cells */
.tableCell {
    font-size: 0.9rem;
    font-weight: 600;
    color: #374151;
    padding: 16px 12px;
    border-bottom: 1px solid #e5e7eb;
    display: flex;
    align-items: center;
}

.tableIcon {
    margin-right: 8px;
    color: #7ca2e1;
}

/* Next button */
.nextBtn {
    position: absolute;
    top: -40px;
    right: 20px;
    padding: 0.60rem 2rem;
    font-size: 1.1rem;
    color: #fff;
    background: rgba(0, 0, 0, 0.6);
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background 0.3s ease, box-shadow 0.3s ease;
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3);
}

.nextBtn:hover {
    background: linear-gradient(135deg, #f2f2f2 -20%, #7ca2e1);
    transform: scale(1.05);
}

.nextIcon {
    margin-left: 10px;
    transition: transform 0.3s ease;
}

.nextBtn:hover .nextIcon {
    transform: translateX(5px);
}

/* Responsive design */
@media (max-width: 768px) {
    .tableCell {
        white-space: normal;
    }

    .formHead {
        font-size: 1.5rem;
    }

    .nextBtn {
        top: -30px;
        right: 15px;
    }
}
