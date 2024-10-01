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



.container {
    display: flex;
    justify-content: space-between;
    padding: 20px;
    gap: 20px;
}

.uploadDocuments {
    flex: 1.5; /* Larger than the sidebar */
    background: rgba(0, 0, 0, 0.5);
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);
}

.documentHead {
    text-align: center;
    font-size: 2rem;
    font-weight: bold;
    color: #fff;
    margin-bottom: 2rem;
    margin: 0;
    padding-bottom: 1rem;
}

.reviewSection {
    display: flex;
    flex-direction: column;
}

.noFile {
    color: #666;
    font-style: italic;
}

/*.formDisplay {*/
/*    flex: 1;*/
/*    display: flex;*/
/*    justify-content: center;*/
/*    align-items: flex-start;*/
/*    background: rgba(0, 0, 0, 0.5);*/
/*    padding: 20px;*/
/*    border-radius: 10px;*/
/*    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);*/
/*}*/








/* Main container for the form display */
.formDisplay {
    background-color: #1e293b; /* Same dark background as sidebar */
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 12px 30px rgba(0, 0, 0, 0.4); /* Same strong shadow as sidebar */
    flex: 3; /* Flex-grow to take up space proportionally */
    transition: all 0.3s ease; /* Smooth transitions for any future changes */
    max-height: 100%; /* Maintain a fixed height */
    display: flex;
    justify-content: center;
    align-items: center;
    color: #e2e8f0; /* Light text color like the sidebar */
    overflow: auto; /* Handle overflow in case content exceeds container height */
}

/* Style the forms inside the formDisplay */
.formDisplay > * {
    margin-bottom: 20px;
    width: 100%; /* Ensure forms take full width */
}

/* Text when no form is selected */
.formDisplay p {
    color: #94a3b8; /* Muted blue-grey text color */
    text-align: center;
    font-size: 1.2rem;
    padding: 40px 0;
}

/* Responsive behavior for smaller screens */
@media (max-width: 768px) {
    .formDisplay {
        padding: 15px;
        max-width: 100%; /* Adjust width for mobile */
        flex: none; /* Override flex-grow behavior for small screens */
    }

    .formDisplay p {
        font-size: 1rem;
    }
}
