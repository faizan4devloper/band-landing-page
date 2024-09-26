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
                    <div className={styles.previewContainer}>
                        <div className={styles.preview}>
                            {allDocuments.map((doc, index) => (
                                <div key={index} className={styles.previewItem}>
                                    <p className={styles.documentType}>Type: {getDocumentType(doc)}</p>
                                    {/* Handle uploaded file (Blob object) and existing document (URL string) differently */}
                                    {doc.type?.startsWith('image/') || doc.url?.endsWith('.jpg') || doc.url?.endsWith('.png') ? (
                                        <img
                                            src={doc.url ? doc.url : URL.createObjectURL(doc)}
                                            alt={doc.name || "Document Preview"}
                                            className={styles.imagePreview}
                                        />
                                    ) : doc.type === 'application/pdf' || doc.url?.endsWith('.pdf') ? (
                                        <iframe
                                            src={`${doc.url ? doc.url : URL.createObjectURL(doc)}#toolbar=0&navpanes=0&scrollbar=0`}
                                            title={`PDF Preview ${index}`}
                                            className={styles.pdfPreview}
                                            width="100%"
                                            height="750px"
                                            frameBorder="0"
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
            </div>

            {/* Render the forms */}
            <div className={styles.formsContainer}>
                <PaymentInstructionForm formData={formData} />
                <PaymentDetails paymentData={paymentData} />
                <LostPolicyForm formData={lostPolicyFormData} />
                <WitnessDetails witnessData={witnessData} />
            </div>
        </div>
    );
};

export default UploadDocuments;


.container {
    display: flex;
    flex-wrap: wrap;
    gap: 2rem;
    justify-content: space-around;
    align-items: flex-start;
    padding: 2rem;
}

.uploadDocuments {
    flex: 1 1 45%;
    background: rgba(0, 0, 0, 0.6);
    backdrop-filter: blur(10px);
    border-radius: 10px;
    padding: 1.5rem;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);
}

.formsContainer {
    flex: 1 1 45%;
    display: grid;
    grid-template-columns: 1fr;
    gap: 1.5rem;
}

.documentHead {
    color: #fff;
    text-align: center;
    margin-bottom: 1.5rem;
    font-size: 2rem;
    font-weight: 600;
}

.previewContainer {
    max-width: 100%;
    overflow: hidden;
}

.previewItem {
    margin-bottom: 1.5rem;
    background-color: #fff;
    padding: 1rem;
    border-radius: 10px;
    box-shadow: 0 4px 16px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s ease;
}

.previewItem:hover {
    transform: translateY(-5px);
}

.imagePreview, .pdfPreview {
    max-width: 100%;
    height: auto;
    border-radius: 10px;
    box-shadow: 0 4px 16px rgba(0, 0, 0, 0.1);
}

.noFile {
    color: #ccc;
    text-align: center;
}

.formsContainer > div {
    background-color: #fff;
    padding: 1.5rem;
    border-radius: 10px;
    box-shadow: 0 4px 16px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s ease;
}

.formsContainer > div:hover {
    transform: translateY(-5px);
}
