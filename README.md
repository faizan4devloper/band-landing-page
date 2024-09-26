import React from 'react';
import { useLocation } from 'react-router-dom';
import styles from './UploadDocuments.module.css';
import PaymentInstructionForm from './PaymentInstructionForm';
import PaymentDetails from './PaymentDetails';
import LostPolicyForm from './LostPolicyForm';
import WitnessDetails from './WitnessDetails';
import data from './documentEntities.json'; // Import the JSON file

const UploadDocuments = () => {
    const location = useLocation();
    const { uploadedFile, documents = [] } = location.state || {};

    // Combine uploaded file and existing documents into a single array
    const allDocuments = [
        ...(uploadedFile ? [uploadedFile] : []),
        ...documents
    ];

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
            {/* Here you can render the imported forms/components */}
            <PaymentInstructionForm />
            <PaymentDetails />
            <LostPolicyForm />
            <WitnessDetails />
            {/* Example of using the imported data */}
            <div>
                <h3>Document Entities</h3>
                <pre>{JSON.stringify(data, null, 2)}</pre> {/* Render the JSON data for debugging */}
            </div>
        </div>
    );
};

export default UploadDocuments;
