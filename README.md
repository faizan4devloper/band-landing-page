
import React, { useState } from 'react';
import { useLocation } from 'react-router-dom';
import Sidebar from './Sidebar';
import FormDisplay from './FormDisplay';
import styles from './NewPage.module.css';

const NewPage = () => {
    const location = useLocation();
    const { uploadedFile, documents = [] } = location.state || {};

    const allDocuments = [
        ...(uploadedFile ? [uploadedFile] : []),
        ...documents
    ];

    // No need to extract data here since FormDisplay uses the JSON directly.
    const [selectedForm, setSelectedForm] = useState(null);

    return (
        <div className={styles.container}>
            
            
            {/* Render the FormDisplay component, passing only the selectedForm */}
            <Sidebar onSelectForm={setSelectedForm} />
            
            {/* Render the Sidebar component, passing the setSelectedForm function */}
            <FormDisplay selectedForm={selectedForm} />
        </div>
    );
};

export default NewPage;



import React, { useEffect, useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronRight } from '@fortawesome/free-solid-svg-icons';
import formDataJson from '../../../Json/DocumentEntities.json'; // Import the JSON file
import styles from './FormDisplay.module.css';
 
const FormDisplay = ({ selectedForm }) => {
    const [formData, setFormData] = useState(null);
 
    useEffect(() => {
        // Simulate fetching data from a JSON file
        setFormData(formDataJson.extracted_data);
    }, []);
 
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
                
                <button className={styles.nextBtn}>Next <FontAwesomeIcon icon={faChevronRight} className={styles.nextIcon}/></button>
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
i have NewPage.js page i want after user click next button from FormDisplay then user go to the NewPage.js 
