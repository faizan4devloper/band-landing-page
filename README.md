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
            <div className={styles.contentWrapper}>
                {/* Left side: Document preview section */}
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
                                                src={`${doc.url ? doc.url : URL.createObjectURL(doc)}#toolbar=0&navpanes=0&scrollbar=0`}
                                                title={`PDF Preview ${index}`}
                                                className={styles.pdfPreview}
                                                width="550px"
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
                
                {/* Right side: Forms container */}
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



.container {
    display: flex;
    justify-content: center;
    padding: 3rem;
    border-radius: 12px;
}

.contentWrapper {
    display: flex;
    justify-content: space-between;
    width: 90%;
    background: rgba(0, 0, 0, 0.5);
    backdrop-filter: blur(10px);
    border-radius: 8px;
    color: #fff;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);
    padding: 2rem;
    gap: 2rem;
}

.uploadDocuments {
    flex: 1;
    max-width: 60%;
}

.formsContainer {
    flex: 1;
    display: grid;
    grid-template-columns: 1fr;
    gap: 2rem;
    background-color: #fff;
    padding: 1.5rem;
    box-shadow: 0 6px 16px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s ease;
}

.formsContainer > div:hover {
    transform: translateY(-8px);
}

.preview {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 1.5rem;
}

.previewItem {
    background-color: #ffffff;
    border-radius: 12px;
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.05);
    padding: 1.5rem;
    text-align: center;
    transition: transform 0.3s ease-in-out;
}

.previewItem:hover {
    transform: translateY(-10px);
}

.documentType {
    font-size: 1rem;
    font-weight: bold;
    color: #2c3e50;
    margin-bottom: 1rem;
}

.imagePreview, .pdfPreview {
    max-width: 100%;
    border-radius: 8px;
    margin-bottom: 1rem;
}

.documentLink {
    color: #3498db;
    text-decoration: none;
    font-weight: bold;
    border: 2px solid #3498db;
    padding: 0.5rem 1rem;
    border-radius: 8px;
    transition: background-color 0.3s ease, color 0.3s ease;
}

.documentLink:hover {
    background-color: #3498db;
    color: #fff;
}

.noFile {
    color: #888;
    font-size: 1.2rem;
    text-align: center;
    margin-top: 2rem;
}

@media (max-width: 768px) {
    .contentWrapper {
        flex-direction: column;
    }

    .uploadDocuments {
        width: 100%;
    }

    .formsContainer {
        grid-template-columns: 1fr;
    }
}
