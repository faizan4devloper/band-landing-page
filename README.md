import React, { useState, useMemo } from 'react';
import { useLocation } from 'react-router-dom';
import PaymentInstructionForm from './PaymentInstructionForm';
import PaymentDetails from './PaymentDetails';
import LostPolicyForm from './LostPolicyForm';
import WitnessDetails from './WitnessDetails';
import styles from './UploadDocuments.module.css';

const UploadDocuments = () => {
    const location = useLocation();
    const { uploadedFile, documents = [] } = location.state || {};
    const allDocuments = [...(uploadedFile ? [uploadedFile] : []), ...documents];

    const [selectedForm, setSelectedForm] = useState(null);

    // Memoized document preview to prevent unnecessary re-renders
    const documentPreview = useMemo(() => {
        return allDocuments.length > 0 ? (
            <div className={styles.reviewSection}>
                <div className={styles.preview}>
                    {allDocuments.map((doc, index) => (
                        <div key={index} className={`${styles.previewItem} ${styles.active}`}>
                            <p className={styles.documentType}>Type: {getDocumentType(doc)}</p>
                            {doc.type?.startsWith('image/') || doc.url?.endsWith('.jpg') || doc.url?.endsWith('.png') ? (
                                <img
                                    src={doc.url ? doc.url : URL.createObjectURL(doc)}
                                    alt={doc.name || 'Document Preview'}
                                    className={styles.imagePreview}
                                />
                            ) : doc.type === 'application/pdf' || doc.url?.endsWith('.pdf') ? (
                                <iframe
                                    src={`${doc.url ? doc.url : URL.createObjectURL(doc)}#toolbar=0&navpanes=0&scrollbar=0`}
                                    title={`PDF Preview ${index}`}
                                    className={styles.pdfPreview}
                                    width="400px"
                                    height="550px"
                                    frameBorder="0"
                                />
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
        );
    }, [allDocuments]); // Only update when documents change

    // Memoized form rendering to avoid unnecessary re-renders
    const renderFormData = useMemo(() => {
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
    }, [selectedForm]);

    return (
        <div className={styles.container}>
            <div className={styles.uploadDocuments}>
                <h2 className={styles.documentHead}>Documents Review</h2>
                {documentPreview}
            </div>

            <div className={`${styles.formDisplay} ${selectedForm ? styles.active : ''}`}>
                {renderFormData}
            </div>

            <div className={styles.sidebar}>
                <h3 className={styles.sidebarTitle}>Forms</h3>
                <ul className={styles.formList}>
                    <li onClick={() => setSelectedForm('PaymentInstructionForm')} className={styles.formItem}>
                        Payment Instruction Form
                    </li>
                    <li onClick={() => setSelectedForm('PaymentDetails')} className={styles.formItem}>
                        Payment Details
                    </li>
                    <li onClick={() => setSelectedForm('LostPolicyForm')} className={styles.formItem}>
                        Lost Policy Form
                    </li>
                    <li onClick={() => setSelectedForm('WitnessDetails')} className={styles.formItem}>
                        Witness Details
                    </li>
                </ul>
            </div>
        </div>
    );
};

// Helper function to get document type
const getDocumentType = (doc) => {
    if (doc.type?.startsWith('image/')) return 'Image';
    if (doc.type === 'application/pdf') return 'PDF';
    return 'Document';
};

export default UploadDocuments;


.container {
    display: flex;
    justify-content: space-between;
    padding: 20px;
}

.uploadDocuments {
    flex: 1;
    margin-right: 20px;
}

.documentHead {
    font-size: 24px;
    margin-bottom: 10px;
}

.reviewSection {
    display: flex;
    flex-direction: column;
}

.preview {
    display: flex;
    flex-wrap: wrap;
}

.previewItem {
    width: 400px;
    height: 550px;
    margin-right: 20px;
    margin-bottom: 20px;
    opacity: 0;
    transform: translateY(20px);
    transition: all 0.3s ease-in-out;
}

.previewItem.active {
    opacity: 1;
    transform: translateY(0);
}

.documentType {
    font-weight: bold;
}

.imagePreview {
    width: 100%;
    height: auto;
}

.pdfPreview {
    width: 100%;
    height: 100%;
}

.noFile {
    color: gray;
    font-size: 16px;
}

.formDisplay {
    flex: 1;
    opacity: 0;
    transition: opacity 0.3s ease-in-out;
}

.formDisplay.active {
    opacity: 1;
}

.sidebar {
    width: 250px;
}

.sidebarTitle {
    font-size: 20px;
    margin-bottom: 10px;
}

.formList {
    list-style-type: none;
    padding: 0;
}

.formItem {
    cursor: pointer;
    padding: 10px;
    border: 1px solid #ccc;
    margin-bottom: 5px;
    transition: background-color 0.3s;
}

.formItem:hover {
    background-color: #f0f0f0;
}
