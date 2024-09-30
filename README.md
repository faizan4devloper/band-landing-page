.container {
    display: flex;
    height: 100vh; /* Full height to utilize vertical space */
}

.uploadDocuments {
    flex: 1; /* Allow this section to take the remaining space */
    padding: 20px; /* Add some padding */
}

.reviewSection {
    margin-bottom: 20px; /* Spacing below the document review section */
}

.preview {
    display: flex;
    flex-wrap: wrap; /* Allow multiple previews to wrap */
}

.noFile {
    color: #ff0000; /* Optional: Make no file message noticeable */
}

/* Form display styling */
.formDisplay {
    flex: 1; /* This will take the remaining space */
    overflow: auto; /* Allow scrolling if content exceeds height */
    padding: 20px; /* Padding for better layout */
    border-left: 1px solid #ccc; /* Optional: Separator between sidebar and form */
}




.sidebar {
    width: 250px; /* Fixed width for the sidebar */
    background: #1e293b; /* Darker background for better contrast */
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 12px 30px rgba(0, 0, 0, 0.4);
    transition: all 0.3s ease;
    height: 100vh; /* Full height to occupy the side */
    overflow-y: auto; /* Scroll if items exceed the height */
}





import React, { useState } from 'react';
import { useLocation } from 'react-router-dom';
import Sidebar from './Sidebar';
import FormDisplay from './FormDisplay';
import DocumentPreview from './DocumentPreview';
import styles from './UploadDocuments.module.css';
import data from '../../../Json/DocumentEntities.json';

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
            <Sidebar onSelectForm={setSelectedForm} />
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
                <FormDisplay 
                    selectedForm={selectedForm} 
                    formData={formData} 
                    paymentData={paymentData} 
                    lostPolicyFormData={lostPolicyFormData} 
                    witnessData={witnessData} 
                />
            </div>
        </div>
    );
};

export default UploadDocuments;
