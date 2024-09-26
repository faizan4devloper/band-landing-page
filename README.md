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
            <PaymentInstructionForm formData={formData} />
            <PaymentDetails paymentData={paymentData} />
            <LostPolicyForm formData={lostPolicyFormData} />
            <WitnessDetails witnessData={witnessData} />
        </div>
    );
};

export default UploadDocuments;

/* UploadDocuments.module.css */

.uploadDocuments {
    max-width: 600px;
    margin: 2rem auto;
    padding: 2rem;
    background: rgba(0, 0, 0, 0.5);
    backdrop-filter: blur(10px);
    border-radius: 8px;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);
}
.documentType {
    font-size: 14px;
    font-weight: bold;
    margin-bottom: 5px;
    text-align: center;
    color: #333;
}
.documentHead {
    text-align: center;
    margin:0;
    font-size: 1.8rem;
    /*margin-bottom: 1.5rem;*/
    color: #fff;
}

.reviewSection {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 1.5rem;
}

.reviewItem {
    padding: 1rem;
    background-color: #ffffff;
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
}

.reviewItem strong {
    font-size: 1.1rem;
    margin-bottom: 0.5rem;
    color: #555;
}

.preview {
    margin-top: 1rem;
}

.preview img {
    max-width: 100%;
    height: auto;
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.preview a {
    display: inline-block;
    margin-top: 0.5rem;
    padding: 0.5rem 1rem;
    background-color: #007bff;
    color: white;
    border-radius: 4px;
    text-decoration: none;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    transition: background-color 0.3s ease;
}

.preview a:hover {
    background-color: #0056b3;
}

.noFile {
    color: #888;
    text-align: center;
    font-size: 1rem;
}

@media (max-width: 768px) {
    .uploadDocuments {
        padding: 1rem;
    }

    .reviewItem {
        padding: 0.5rem;
    }

    h2 {
        font-size: 1.5rem;
    }
}


import React from 'react';
import styles from './PaymentInstructionForm.module.css';

const PaymentInstructionForm = ({ formData = {} }) => {
    return (
        <div className={styles.formContainer}>
            <h3>Payment Instruction Form</h3>
            <p><strong>Statement Date:</strong> {formData.STATEMENT_DATE || 'N/A'}</p>
            <p><strong>Policy Number:</strong> {formData.POLICY_NUMBER || 'N/A'}</p>
            <p><strong>Policy On the Life Of:</strong> {formData.POLICY_ON_THE_LIFE_OF || 'N/A'}</p>
            <p><strong>Policy Owner:</strong> {formData.POLICY_OWNER || 'N/A'}</p>
        </div>
    );
};

export default PaymentInstructionForm;


    import React from 'react';
    import styles from './PaymentDetails.module.css';
    
    const PaymentDetails = ({ paymentData = {}}) => {
        return (
            <div className={styles.paymentContainer}>
                <h3>Payment Details</h3>
                <p><strong>Bank Name & Address:</strong> {paymentData.BANK_NAME_AND_ADDRESS || 'N/A'}</p>
                <p><strong>Account Holder's Name:</strong> {paymentData.ACCOUNT_HOLDERS_NAME || 'N/A'}</p>
                <p><strong>Account Number:</strong> {paymentData.ACCOUNT_NUMBER || 'N/A'}</p>
                <p><strong>Bank Sort Code:</strong> {paymentData.BANK_SORT_CODE || 'N/A'}</p>
                <p><strong>Signed Full Name:</strong> {paymentData.SIGNED_FULL_NAME || 'N/A'}</p>
                <p><strong>Signed Date:</strong> {paymentData.SIGNED_DATE || 'N/A'}</p>
            </div>
        );
    };
    
    export default PaymentDetails;

        import React from 'react';
    import styles from './LostPolicyForm.module.css';
    
    const LostPolicyForm = ({ lostPolicyFormData = {} }) => {
        return (
            <div className={styles.formContainer}>
                <h3>Lost Policy Form</h3>
                <p><strong>Statement Date:</strong> {lostPolicyFormData.STATEMENT_DATE || 'N/A'}</p>
                <p><strong>Policy Number:</strong> {lostPolicyFormData.POLICY_NUMBER || 'N/A'}</p>
                <p><strong>Policy On the Life Of:</strong> {lostPolicyFormData.POLICY_ON_THE_LIFE_OF || 'N/A'}</p>
                <p><strong>Policy Owner:</strong> {lostPolicyFormData.POLICY_OWNER || 'N/A'}</p>
            </div>
        );
    };
    
    export default LostPolicyForm;

    import React from 'react';
import styles from './WitnessDetails.module.css';

const WitnessDetails = ({ witnessData ={} }) => {
    return (
        <div className={styles.witnessContainer}>
            <h3>Witness Details</h3>
            <p><strong>Full Name of Witness:</strong> {witnessData.FULL_NAME_OF_WITNESS || 'N/A'}</p>
            <p><strong>Date:</strong> {witnessData.DATE || 'N/A'}</p>
            <p><strong>Address of Witness:</strong> {witnessData.ADDRESS_OF_WITNESS || 'N/A'}</p>
            <p><strong>Day-time Telephone:</strong> {witnessData.DAY_TIME_TELEPHONE_NUMBER_OF_WITNESS || 'N/A'}</p>
            <p><strong>Occupation:</strong> {witnessData.OCCUPATION_OF_WITNESS || 'N/A'}</p>
        </div>
    );
};

export default WitnessDetails;
