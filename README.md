
import React, { useEffect, useState } from 'react';
import { useNavigate } from 'react-router-dom'; // Import useNavigate for navigation
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowRight } from '@fortawesome/free-solid-svg-icons';
import formDataJson from '../../../Json/DocumentEntities.json'; // Import the JSON file
import styles from './FormDisplay.module.css';

const FormDisplay = ({ selectedForm }) => {
    const [formData, setFormData] = useState(null);
    const navigate = useNavigate(); // Create navigate function

    useEffect(() => {
        // Simulate fetching data from a JSON file
        setFormData(formDataJson.extracted_data);
    }, []);

    // Function to handle "Next" button click
    const handleNextClick = () => {
        // Navigate to the Verification page
        navigate('/New-page');
    };

    // Render form data based on selected form in a table format
    const renderFormData = () => {
        if (!formData) {
            return <p className={styles.loadingMessage}>Loading data...</p>;
        }

        const selectedData = formData[selectedForm];

        if (!selectedData) {
            return <p className={styles.selectMessage}>Please select a form to view its data.</p>;
        }

        return (
            <div className={styles.tableContainer}>
                <h2 className={styles.formHead}>{selectedForm.replace(/_/g, ' ')}</h2>

                <button className={styles.nextBtn} onClick={handleNextClick}> {/* Add onClick */}
                    <FontAwesomeIcon icon={faArrowRight} className={styles.nextIcon}/>
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
                                    {key.replace(/_/g, ' ')}
                                </td>
                                <td className={styles.tableCell}>
                                    {value}
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
    color: #000;
    text-align: left;
    margin-bottom: 25px;
    text-transform: capitalize;
    letter-spacing: 1px; /* Slightly increasing letter spacing */
}

/* Table container */
.tableContainer {
    width: 100%;
    height: 100%;
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
    box-shadow: 0 6px 20px rgba(0, 0, 0, 0.1);
    overflow: hidden;
    background-color: #ffffff;
}

/* Table headers */
.tableHeader {
    font-size: 1rem;
    font-weight: bold;
    color: #ffffff; /* White for headers */
    background: linear-gradient(135deg, #4a90e2, #1c3c8b); /* Gradient background */
    padding: 16px 12px;
    text-align: center;
    border-bottom: 2px solid #e5e7eb;
    text-transform: capitalize;
    white-space: nowrap;
    letter-spacing: 0.8px;
}

/* Table rows */
.tableRow {
    transition: background-color 0.3s ease, transform 0.2s ease;
    cursor: pointer;
}

.tableRow:hover {
    background-color: rgba(124, 162, 225, 0.1); /* Light hover effect */
    transform: scale(1.01); /* Slight zoom on hover */
}

/* Table cells */
.tableCell {
    font-size: 0.85rem;
    font-weight: 600;
    color: #374151;
    padding: 16px 12px;
    border-bottom: 1px solid #e5e7eb;
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
    max-width: 250px;
    border-radius: 6px;
    transition: background-color 0.3s ease, transform 0.3s ease;
}

.tableCell:hover {
    background-color: #e1e9f5; /* Highlighted background on hover */
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    transform: scale(1.02); /* Slight zoom effect */
    cursor: pointer;
}

/* Alternating row colors */
.tableRow:nth-child(even) {
    background-color: #f9fafb; /* Light background for alternating rows */
}

.tableRow:nth-child(odd) {
    background-color: #ffffff;
}

/* Table borders */
.dataTable th,
.dataTable td {
    border-bottom: 1px solid #e9e9e9; /* Subtle borders between rows */
}

/* Next button */
.nextBtn {
    position: absolute;
    top: -40px;
    right: 40px;
    align-items: center;
    padding: 6px 12px;
    font-size: 1rem;
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
    background: linear-gradient(135deg, #F2F2F2 -20%, #7ca2e1);
    transform: scale(1.05);
}

/* Mobile responsiveness */
@media (max-width: 768px) {
    .dataTable {
        display: block;
        width: 100%;
    }

    .tableCell {
        white-space: normal; /* Wrap text on smaller screens */
    }

    .tableHeader,
    .tableCell {
        padding: 10px;
        font-size: 1rem;
    }

    .formHead {
        font-size: 1.8rem;
    }
}
