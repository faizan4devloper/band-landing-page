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

                <button className={styles.nextBtn} onClick={handleNextClick}>
                    <FontAwesomeIcon icon={faArrowRight} className={styles.nextIcon} />
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
    font-size: 1.1rem;
    font-weight: bold;
    color: #000;
    text-align: center;
    text-transform: capitalize;
}

/* Table container */
.tableContainer {
    width: 100%;
    height: 100%;
    background-color: #f1f5f9;
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
    border-radius: 6px;
    overflow: hidden;
    background-color: #ffffff;
    font-family: 'Roboto', sans-serif;
}

/* Table headers */
.tableHeader {
    font-size: 1rem;
    font-weight: bold;
    color: #000;
    padding: 14px 12px;
    text-align: left;
    border-bottom: 2px solid #e5e7eb;
    text-transform: capitalize;
    white-space: nowrap;
}

/* Table rows */
.tableRow:nth-child(even) {
    background-color: #f9fafb;
}

.tableRow:nth-child(odd) {
    background-color: #ffffff;
}

/* Table cells */
.tableCell {
    font-size: .9rem;
    font-weight: 600;
    color: #374151;
    padding: 16px 12px;
    border-bottom: 1px solid #e5e7eb;
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
    max-width: 250px;
    cursor: pointer;
    transition: background-color 0.3s ease, box-shadow 0.3s ease, transform 0.3s ease;
}

.tableCell:hover {
    background-color: #f4f4f4;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    transform: scale(1.02);
}

/* Next Button */
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
}

.nextBtn:hover {
    background: linear-gradient(135deg, #F2F2F2 -20%, #7ca2e1);
    transform: scale(1.05);
}

.nextIcon {
    transition: transform 0.3s ease;
}

.nextBtn:hover .nextIcon {
    transform: translateX(5px);
}

/* Mobile responsiveness */
@media (max-width: 768px) {
    .dataTable {
        display: block;
        width: 100%;
    }

    .tableCell {
        white-space: normal;
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
