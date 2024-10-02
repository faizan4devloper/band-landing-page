// UploadDocuments.js
import React, { useState } from 'react';
import { useLocation } from 'react-router-dom';
import Sidebar from './Sidebar';
import FormDisplay from './FormDisplay';
// import DocumentPreview from './DocumentPreview';
import styles from './UploadDocuments.module.css';
import data from '../../../Json/DocumentEntities.json'; // Import the JSON file

const UploadDocuments = () => {
    const location = useLocation();
    const { uploadedFile, documents = [] } = location.state || {};

    const allDocuments = [
        ...(uploadedFile ? [uploadedFile] : []),
        ...documents
    ];

    const formData = data.extracted_data?.PAYMENT_INSTRUCTION_FORM || {};
    const paymentData = data.extracted_data?.PAYMENT_DETAILS || {};
    const lostPolicyFormData = data.extracted_data?.LOST_POLICY_FORM || {};
    const witnessData = data.extracted_data?.LOST_POLICY_FORM_WITNESSED_BY || {};

    const [selectedForm, setSelectedForm] = useState(null);

    return (
        <div className={styles.container}>
            

            <FormDisplay 
                selectedForm={selectedForm} 
                formData={formData} 
                paymentData={paymentData} 
                lostPolicyFormData={lostPolicyFormData} 
                witnessData={witnessData} 
            />

            <Sidebar onSelectForm={setSelectedForm} />
        </div>
    );
};

export default UploadDocuments;
