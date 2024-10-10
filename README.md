
import React, { useEffect, useState } from 'react';
import { useNavigate } from 'react-router-dom'; // Import useNavigate for navigation
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronRight } from '@fortawesome/free-solid-svg-icons';
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
                <h2 className={styles.formHead}>{selectedForm.replace(/_/g, ' ')}</h2>

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



/* Main container */
.formDisplay {
    background: linear-gradient(135deg, #1e293b, #334155);
    padding: 30px;
    /*border-radius: 16px;*/
    /*box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);*/
    height: 100%;
    width: 100%;
    overflow-y: auto;
    display: flex;
    /*flex-direction: column;*/
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
    font-size: 1.2rem;
    font-weight: bold;
    color: #000;
    text-align: left;
    margin-bottom: 25px;
    text-transform: capitalize;
}
 
/* Table container */
.tableContainer {
    margin-top: 20px;
    width: 100%;
    background-color: #f1f5f9;
    border-left: 4px solid #7ca2e1;
    /*border-radius: 16px;*/
    padding: 20px;
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
    overflow-x: auto; /* Makes the table scrollable on small screens */
}
 
/* Table styles */
.dataTable {
    width: 100%;
    border-collapse: separate;
    border-spacing: 0;
    /*border-radius: 12px;*/
    overflow: hidden;
    background-color: #ffffff;
}
 
/* Table headers */
.tableHeader {
    font-size: 1rem;
    font-weight: bold;
    color: #1f2937; /* Dark gray for headers */
    background-color: #f9fafb; /* Light gray for header background */
    padding: 16px 12px;
    text-align: left;
    border-bottom: 2px solid #e5e7eb;
    text-transform: capitalize;
    white-space: nowrap; /* Prevents headers from wrapping */
}
 
/* Table rows */
.tableRow:nth-child(even) {
    background-color: #f9fafb; /* Light background for alternating rows */
}
 
.tableRow:nth-child(odd) {
    background-color: #ffffff;
}
 
/* Table cells */
.tableCell {
    font-size: .7rem;
    font-weight: 600;
    color: #374151; /* Medium gray for text */
    padding: 16px 12px;
    border-bottom: 1px solid #e5e7eb;
    white-space: nowrap;
    text-overflow: ellipsis; /* Makes long text look good */
    overflow: hidden; /* Hides overflow for long text */
    max-width: 250px;
}

/*.nextBtn{*/
/*    position: absolute;*/
/*    padding: 12px 15px 12px 15px;*/
/*    top: -40px;*/
/*    right: 30px;*/
/*}*/

/* NextButton.css */
.nextBtn {
    position: absolute;
    top: -40px;
    right: 20px;
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
  background:linear-gradient(135deg, #F2F2F2 -20%, #7ca2e1);
  transform: scale(1.05);
}
 
.nextIcon {
  margin-left: 10px;
  transition: transform 0.3s ease;
}
 
.nextBtn:hover .nextIcon {
  transform: translateX(5px);
}

/* Hover effect for table rows */
/*
 
/* Table borders */
.dataTable th,
.dataTable td {
    
    border-bottom: 1px solid #e9e9e9; /* Subtle borders between rows */
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
