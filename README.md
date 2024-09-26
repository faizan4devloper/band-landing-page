import React from 'react';
import { useLocation } from 'react-router-dom';
import styles from './UploadDocuments.module.css';
import PaymentInstructionForm from '../Entities/PaymentInstructionForm';
import PaymentDetails from '../Entities/PaymentDetails';
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

    // Access JSON data
    const formData = data.paymentInstruction;
    const paymentData = data.paymentDetails;
    const witnessData = data.witnessDetails;

    // Helper function to get document type label
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
        <div className={styles.uploadDocuments}>
            <h2 className={styles.documentHead}>Documents Review</h2>
            {allDocuments.length > 0 ? (
                <div className={styles.reviewSection}>
                    <div className={styles.preview}>
                        {allDocuments.map((doc, index) => (
                            <div key={index} className={styles.previewItem}>
                                <p className={styles.documentType}>Type: {getDocumentType(doc)}</p>
                                {doc.type?.startsWith('image/') || doc.url?.endsWith('.jpg') || doc.url?.endsWith('.png') ? (
                                    <img
                                        src={doc.url ? doc.url : URL.createObjectURL(doc)}
                                        alt={doc.name || "Document Preview"}
                                        className={styles.imagePreview}
                                    />
                                ) : doc.type === 'application/pdf' || doc.url?.endsWith('.pdf') ? (
                                    <iframe
                                        src={doc.url ? doc.url : URL.createObjectURL(doc)}
                                        title={`PDF Preview ${index}`}
                                        className={styles.pdfPreview}
                                        width="550px"
                                        height="750px"
                                    />
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
            {/* Render the imported forms/components */}
            <PaymentInstructionForm formData={formData} />
            <PaymentDetails paymentData={paymentData} />
            <WitnessDetails witnessData={witnessData} />
        </div>
    );
};

export default UploadDocuments;


import React from 'react';
import styles from './PaymentInstructionForm.module.css';

const PaymentInstructionForm = ({ formData }) => {
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

const PaymentDetails = ({ paymentData }) => {
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
import styles from './WitnessDetails.module.css';

const WitnessDetails = ({ witnessData }) => {
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

