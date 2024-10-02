import React, { useState } from 'react';
import { useLocation } from 'react-router-dom';
import Sidebar from './Sidebar';
import FormDisplay from './FormDisplay';
import styles from './UploadDocuments.module.css';
import data from '../../../Json/DocumentEntities.json'; // Import the JSON file

const UploadDocuments = () => {
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
            <FormDisplay selectedForm={selectedForm} />
            
            {/* Render the Sidebar component, passing the setSelectedForm function */}
            <Sidebar onSelectForm={setSelectedForm} />
        </div>
    );
};

export default UploadDocuments;


import React from 'react';
import styles from './Sidebar.module.css';

const Sidebar = ({ onSelectForm }) => {
    const handleFormSelect = (formName) => {
        onSelectForm(formName);
    };

    return (
        <div className={styles.sidebar}>
            <h3>Select a Form</h3>
            <ul>
                <li onClick={() => handleFormSelect('PAYMENT_INSTRUCTION_FORM')}>Payment Instruction Form</li>
                <li onClick={() => handleFormSelect('PAYMENT_DETAILS')}>Payment Details</li>
                <li onClick={() => handleFormSelect('LOST_POLICY_FORM')}>Lost Policy Form</li>
                <li onClick={() => handleFormSelect('LOST_POLICY_FORM_WITNESSED_BY')}>Witness Details</li>
            </ul>
        </div>
    );
};

export default Sidebar;
