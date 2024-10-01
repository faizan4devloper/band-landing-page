/* Main container for the entire UploadDocuments page */
.container {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: flex-start;
    max-width: 100%;
    height: 100vh; /* Full viewport height */
    gap: 20px; /* Space between sidebar, form, and documents */
    padding: 20px;
}

/* Document review section styling */
.uploadDocuments {
    flex: 2; /* Take up twice the space compared to the sidebar */
    background-color: #f8fafc;
    border-radius: 12px;
    box-shadow: 0 12px 30px rgba(0, 0, 0, 0.2);
    padding: 20px;
    height: 100%; /* Full height */
    overflow-y: auto;
}

/* Headings */
.documentHead {
    font-size: 1.8rem;
    font-weight: bold;
    margin-bottom: 20px;
    color: #334155;
    text-align: center;
}

/* Review section for documents */
.reviewSection {
    display: flex;
    flex-direction: column;
}

/* No file message */
.noFile {
    color: #64748b;
    font-size: 1.2rem;
    text-align: center;
}

/* Preview section for individual documents */
.preview {
    display: flex;
    flex-wrap: wrap;
    gap: 15px;
}

/* Individual document preview styling */
.preview > * {
    background-color: #e2e8f0;
    border-radius: 8px;
    padding: 10px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    transition: all 0.3s ease;
}

/* When hovering over individual document preview */
.preview > *:hover {
    transform: scale(1.05);
}

/* Sidebar styling from previous code */
.sidebar {
    flex: 1; /* Sidebar takes up less space */
    max-width: 250px; /* Fixed width for the sidebar */
    height: 100%; /* Full height */
}

/* FormDisplay styling from previous code */
.formDisplay {
    flex: 3; /* Form takes up more space than the sidebar */
    height: 100%; /* Full height */
    max-width: 600px; /* Optional, to limit form width */
    overflow-y: auto; /* Scroll if content exceeds height */
}


// UploadDocuments.js
import React, { useState } from 'react';
import { useLocation } from 'react-router-dom';
import Sidebar from './Sidebar';
import FormDisplay from './FormDisplay';
import DocumentPreview from './DocumentPreview';
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
            <div className={styles.uploadDocuments}>
                <h2 className={styles.documentHead}>Documents Review</h2>
                {allDocuments.length > 0 ? (
                    <div className={styles.reviewSection}>
                        <div className={styles.preview}>
                            {allDocuments.map((doc, index) => (
                                <DocumentPreview key={index} document={doc} />
                            ))}
                        </div>
                    </div>
                ) : (
                    <p className={styles.noFile}>No document available</p>
                )}
            </div>

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
