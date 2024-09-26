import React from 'react';
import { useLocation } from 'react-router-dom';
import styles from './UploadDocuments.module.css';
import PaymentInstructionForm from '../Entities/PaymentInstructionForm';
import PaymentDetails from '../Entities/PaymentDetails';
import LostPolicyForm from '../Entities/LostPolicyForm';
import WitnessDetails from '../Entities/WitnessDetails';
import data from '../../../Json/DocumentEntities.json'; // Import the JSON file

const UploadDocuments = () => {
    const location = useLocation();
    const { uploadedFile, documents = [] } = location.state || {};

    // Combine uploaded file and existing documents into a single array
    const allDocuments = [
        ...(uploadedFile ? [uploadedFile] : []),
        ...documents
    ];

    // Access JSON data with fallback to handle undefined
    const formData = data.extracted_data?.PAYMENT_INSTRUCTION_FORM || {};
    const paymentData = data.extracted_data?.PAYMENT_DETAILS || {};
    const lostPolicyFormData = data.extracted_data?.LOST_POLICY_FORM || {};
    const witnessData = data.extracted_data?.LOST_POLICY_FORM_WITNESSED_BY || {};


    // Helper function to get document type label
    const getDocumentType = (doc) => {
        if (doc.type?.startsWith('image/') || doc.url?.endsWith('.jpg') || doc.url?.endsWith('.png')) {
            return 'Image';
        }
        else if (doc.type === 'application/pdf' || doc.url?.endsWith('.pdf')) {
            return 'PDF';
        }
        else if (doc.type === 'application/vnd.openxmlformats-officedocument.wordprocessingml.document' || doc.url?.endsWith('.docx')) {
            return 'DOCX';
        }
        else {
            return 'Other';
        }
    };

    return (
    <div className={styles.container}>    
        <div className={styles.uploadDocuments}>
            <h2 className={styles.documentHead}>Documents Review</h2>
            {allDocuments.length > 0 ? (
                <div className={styles.reviewSection}>
                    <div className={styles.preview}>
                        {allDocuments.map((doc, index) => (
                            <div key={index} className={styles.previewItem}>
                                <p className={styles.documentType}>Type: {getDocumentType(doc)}</p> {/* Display document type */}
                                {/* Handle uploaded file (Blob object) and existing document (URL string) differently */}
                                {doc.type?.startsWith('image/') || doc.url?.endsWith('.jpg') || doc.url?.endsWith('.png') ? (
                                    <img
                                        src={doc.url ? doc.url : URL.createObjectURL(doc)}
                                        alt={doc.name || "Document Preview"}
                                        className={styles.imagePreview}
                                    />
                                ) : doc.type === 'application/pdf' || doc.url?.endsWith('.pdf') ? (
                                    <iframe
                                        // src={doc.url ? doc.url : URL.createObjectURL(doc)}
                                        src={`${doc.url ? doc.url : URL.createObjectURL(doc)}#toolbar=0&navpanes=0&scrollbar=0`}
                                        title={`PDF Preview ${index}`}
                                        className={styles.pdfPreview}
                                        width="550px"
                                        height="750px"
                                        // sandbox="allow-scripts allow same-origin"
                                        frameBorder="0"
                                    />
                                    // <object
                                    //     data={doc.url ? doc.url : URL.createObjectURL(doc)}
                                    //     type="application/pdf"
                                    //     className={styles.pdfPreview}
                                    //     width="550px"
                                    //     height="750px"
                                    // />
                                ) : doc.type === 'application/vnd.openxmlformats-officedocument.wordprocessingml.document' || doc.url?.endsWith('.docx') ? (
                                    <a
                                        href={doc.url ? doc.url : URL.createObjectURL(doc)}
                                        download={doc.name || doc.name}
                                        target="_blank"
                                        rel="noopener noreferrer"
                                        className={styles.documentLink}
                                    >
                                        View Document (DOCX)
                                    </a>
                                ) : (
                                    <a
                                        href={doc.url ? doc.url : URL.createObjectURL(doc)}
                                        download={doc.name || doc.name}
                                        target="_blank"
                                        rel="noopener noreferrer"
                                        className={styles.documentLink}
                                    >
                                        View Document
                                    </a>
                                )}
                            </div>
                        ))}
                    </div>
                </div>
            ) : (
                <p className={styles.noFile}>No document available</p>
            )}
            {/* Here you can render the imported forms/components */}
            <div className={styles.formsContainer}>
                <PaymentInstructionForm formData={formData} />
                <PaymentDetails paymentData={paymentData} />
                <LostPolicyForm lostPolicyFormData={lostPolicyFormData} />
                <WitnessDetails witnessData={witnessData} />
            </div>
        </div>
    </div>    
    );
};

export default UploadDocuments;

    
    import React from 'react';
    import styles from './LostPolicyForm.module.css';
    
    const LostPolicyForm = ({ lostPolicyFormData = {} }) => {
        return (
            <div className={styles.lostPolicyContainer}>
                <h3>Lost Policy Form</h3>
                <p><strong>Statement Date:</strong> {lostPolicyFormData.STATEMENT_DATE || 'N/A'}</p>
                <p><strong>Policy Number:</strong> {lostPolicyFormData.POLICY_NUMBER || 'N/A'}</p>
                <p><strong>Policy On the Life Of:</strong> {lostPolicyFormData.POLICY_ON_THE_LIFE_OF || 'N/A'}</p>
                <p><strong>Policy Owner:</strong> {lostPolicyFormData.POLICY_OWNER || 'N/A'}</p>
            </div>
        );
    };
    
    export default LostPolicyForm;

    .lostPolicyContainer {
    background-color: #ffffff;
    padding: 20px;
    margin: 20px 0;
    border-radius: 10px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    max-width: 600px;
}

.lostPolicyContainer h3 {
    color: #333;
    font-size: 1.6rem;
    border-bottom: 2px solid #7ca2e1;
    padding-bottom: 10px;
    margin-bottom: 20px;
}

.lostPolicyContainer p {
    font-size: 1.1rem;
    margin: 10px 0;
    color: #555;
}

strong {
    color: #7ca2e1;
}
