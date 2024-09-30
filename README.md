// DocumentPreview.js
import React from 'react';
import styles from './DocumentPreview.module.css';

const DocumentPreview = ({ document, index }) => {
    const getDocumentType = (doc) => {
        if (doc.type?.startsWith('image/') || doc.url?.endsWith('.jpg') || doc.url?.endsWith('.png')) {
            return 'Image';
        } else if (doc.type === 'application/pdf' || doc.url?.endsWith('.pdf')) {
            return 'PDF';
        } else if (doc.type === 'application/vnd.openxmlformats-officedocument.wordprocessingml.document' || doc.url?.endsWith('.docx')) {
            return 'DOCX';
        } else {
            return 'Other';
        }
    };

    return (
        <div key={index} className={styles.previewItem}>
            <p className={styles.documentType}>Type: {getDocumentType(document)}</p>
            {document.type?.startsWith('image/') || document.url?.endsWith('.jpg') || document.url?.endsWith('.png') ? (
                <img
                    src={document.url ? document.url : URL.createObjectURL(document)}
                    alt={document.name || "Document Preview"}
                    className={styles.imagePreview}
                />
            ) : document.type === 'application/pdf' || document.url?.endsWith('.pdf') ? (
                <iframe
                    src={`${document.url ? document.url : URL.createObjectURL(document)}#toolbar=0&navpanes=0&scrollbar=0`}
                    title={`PDF Preview ${index}`}
                    className={styles.pdfPreview}
                    width="400px"
                    height="550px"
                    frameBorder="0"
                />
            ) : (
                <a
                    href={document.url ? document.url : URL.createObjectURL(document)}
                    download={document.name || document.name}
                    target="_blank"
                    rel="noopener noreferrer"
                    className={styles.documentLink}
                >
                    View Document
                </a>
            )}
        </div>
    );
};

export default DocumentPreview;



// Sidebar.js
import React from 'react';
import styles from './Sidebar.module.css';

const Sidebar = ({ onSelectForm }) => {
    return (
        <div className={styles.sidebar}>
            <h3 className={styles.sidebarTitle}>Forms</h3>
            <ul className={styles.formList}>
                <li onClick={() => onSelectForm('PaymentInstructionForm')} className={styles.formItem}>Payment Instruction Form</li>
                <li onClick={() => onSelectForm('PaymentDetails')} className={styles.formItem}>Payment Details</li>
                <li onClick={() => onSelectForm('LostPolicyForm')} className={styles.formItem}>Lost Policy Form</li>
                <li onClick={() => onSelectForm('WitnessDetails')} className={styles.formItem}>Witness Details</li>
            </ul>
        </div>
    );
};

export default Sidebar;




// FormDisplay.js
import React from 'react';
import PaymentInstructionForm from '../Entities/PaymentInstructionForm';
import PaymentDetails from '../Entities/PaymentDetails';
import LostPolicyForm from '../Entities/LostPolicyForm';
import WitnessDetails from '../Entities/WitnessDetails';
import styles from './FormDisplay.module.css';

const FormDisplay = ({ selectedForm, formData, paymentData, lostPolicyFormData, witnessData }) => {
    const renderFormData = () => {
        switch (selectedForm) {
            case 'PaymentInstructionForm':
                return <PaymentInstructionForm formData={formData} />;
            case 'PaymentDetails':
                return <PaymentDetails paymentData={paymentData} />;
            case 'LostPolicyForm':
                return <LostPolicyForm lostPolicyFormData={lostPolicyFormData} />;
            case 'WitnessDetails':
                return <WitnessDetails witnessData={witnessData} />;
            default:
                return <p>Please select a form to view its data.</p>;
        }
    };

    return (
        <div className={styles.formDisplay}>
            {renderFormData()}
        </div>
    );
};

export default FormDisplay;


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

.formDisplay {
    flex: 1;
    display: flex;
    justify-content: center;
    align-items: flex-start;
    background: rgba(0, 0, 0, 0.5);
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);
}





.previewItem {
    margin-bottom: 20px;
    padding: 10px;
    border: 1px solid #e0e0e0;
    border-radius: 8px;
    background: #f9f9f9;
}

.imagePreview {
    max-width: 100%;
    height: auto;
    border-radius: 8px;
}

.pdfPreview {
    border-radius: 8px;
}

.documentLink {
    text-decoration: none;
    color: #007bff;
    font-weight: bold;
}


.sidebar {
    flex: 1; /* Smaller than the upload section */
    background: rgba(0, 0, 0, 0.5);
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);
}

.sidebarTitle {
    font-size: 1.6rem;
    font-weight: bold;
    margin-bottom: 1rem;
}

.formList {
    list-style: none;
    padding: 0;
}

.formItem {
    cursor: pointer;
    padding: 10px;
    margin-bottom: 10px;
    border-radius: 6px;
    transition: background 0.3s;
    background: #e8f0fe; /* Light blue background */
}

.formItem:hover {
    background: #d1e7fd; /* Slightly darker blue on hover */
}
