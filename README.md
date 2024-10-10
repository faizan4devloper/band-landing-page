import React, { useEffect, useState } from 'react';
import { useNavigate } from 'react-router-dom'; // Import useNavigate for navigation
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronRight } from '@fortawesome/free-solid-svg-icons';
// Import the new JSON data
import formDataJson from '../../../Json/NewEntities.json'; // Change the file name accordingly
import styles from './NewFormDisplay.module.css';

const NewFormDisplay = ({ selectedForm }) => {
    const [formData, setFormData] = useState(null);
    const navigate = useNavigate(); // Create navigate function

    useEffect(() => {
        // Set the formData based on the new JSON structure
        if (formDataJson[selectedForm]) {
            setFormData(formDataJson[selectedForm]);
        } else {
            setFormData(null);
        }
    }, [selectedForm]); // Add selectedForm as a dependency

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

        return (
            <div className={styles.tableContainer}>
                <h2 className={styles.formHead}>{selectedForm.replace(/_/g, ' ')}</h2>

                <button className={styles.nextBtn} onClick={handleNextClick}>
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
                        {Object.entries(formData).map(([key, value]) => (
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

export default NewFormDisplay;


import React, { useState } from 'react';
import styles from './NewSidebar.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronRight } from '@fortawesome/free-solid-svg-icons';

const NewSidebar = ({ onSelectForm }) => {
    const [activeItem, setActiveItem] = useState('')
    
    const handleFormSelect = (formName) => {
        setActiveItem(formName);
        onSelectForm(formName);
    };

    return (
        <div className={styles.sidebar}>
            <h3 className={styles.sidebarTitle}>Select a Form</h3>
            <ul className={styles.formList}>
                <li
                    className={`${styles.formItem} ${activeItem === 'PAYMENT_INSTRUCTION_FORM' ? styles.active : ''}`}
                    onClick={() => handleFormSelect('PAYMENT_INSTRUCTION_FORM')}
                >
                    <span className={styles.formText}>Verification Status</span>
                    <FontAwesomeIcon icon={faChevronRight} className={styles.chevronIcon} />
                </li>
                <li
                    className={`${styles.formItem} ${activeItem === 'PAYMENT_DETAILS' ? styles.active : ''}`}
                    onClick={() => handleFormSelect('PAYMENT_DETAILS')}
                >
                    <span className={styles.formText}>Reviewer Insights</span>
                    <FontAwesomeIcon icon={faChevronRight} className={styles.chevronIcon} />
                </li>
                <li
                    className={`${styles.formItem} ${activeItem === 'LOST_POLICY_FORM' ? styles.active : ''}`}
                    onClick={() => handleFormSelect('LOST_POLICY_FORM')}
                >
                    <span className={styles.formText}>Checklist</span>
                    <FontAwesomeIcon icon={faChevronRight} className={styles.chevronIcon} />
                </li>
                
            </ul>
        </div>
    );
};

export default NewSidebar;
