import React, { useState } from 'react';
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

    // State to manage the selected form
    const [selectedForm, setSelectedForm] = useState(null);

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

    // Render the selected form data
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
        <div className={styles.container}>
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
                )}
            </div>

            <div className={styles.formDisplay}>
                {renderFormData()}
            </div>

            <div className={styles.sidebar}>
                <h3 className={styles.sidebarTitle}>Forms</h3>
                <ul className={styles.formList}>
                    <li onClick={() => setSelectedForm('PaymentInstructionForm')} className={styles.formItem}>Payment Instruction Form</li>
                    <li onClick={() => setSelectedForm('PaymentDetails')} className={styles.formItem}>Payment Details</li>
                    <li onClick={() => setSelectedForm('LostPolicyForm')} className={styles.formItem}>Lost Policy Form</li>
                    <li onClick={() => setSelectedForm('WitnessDetails')} className={styles.formItem}>Witness Details</li>
                </ul>
            </div>
        </div>
    );
};

export default UploadDocuments;


/*.container {*/
/*    display: flex;*/
/*    flex-direction: column;*/
/*    align-items: center;*/
/*    justify-content: center;*/
/*    padding: 3rem;*/
/*    border-radius: 12px;*/
/*}*/

/*.contentWrapper {*/
/*    display: flex;*/
/*    justify-content: space-between;*/
/*    width: 90%;*/
/*    background: rgba(0, 0, 0, 0.5);*/
/*    backdrop-filter: blur(10px);*/
/*    border-radius: 8px;*/
/*    color: #fff;*/
/*    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);*/
/*    padding: 2rem;*/
/*    gap: 2rem;*/
/*}*/

/*.uploadDocuments {*/
/*    background: rgba(0, 0, 0, 0.5);*/
/*    flex: 1;*/
    /*max-width: 90%;*/
/*    width: 800px;*/
/*        padding: 30px;*/
/*        border-radius: 12px;*/
/*        border-left: 5px solid #7ca2e1;*/
/*}*/

/*.documentHead {*/
/*    text-align: center;*/
/*    font-size: 2rem;*/
/*    font-weight: bold;*/
/*    color: #fff;*/
    /*margin-bottom: 2rem;*/
    /*margin: 0;*/
/*    padding-bottom: 1rem;*/
/*}*/

/*.reviewSection {*/
/*    display: flex;*/
/*    flex-direction: column;*/
        

/*    align-items: center;*/
/*}*/

/*.preview {*/
/*    display: grid;*/
/*    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));*/
/*    gap: 1.5rem;*/
/*}*/

/*.previewItem {*/
/*    background-color: #ffffff;*/
/*    border-radius: 12px;*/
/*    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.05);*/
/*    padding: 1.5rem;*/
/*    text-align: center;*/
/*    transition: transform 0.3s ease-in-out;*/
/*}*/

/*.previewItem:hover {*/
/*    transform: translateY(-10px);*/
/*}*/

/*.documentType {*/
/*    font-size: 1rem;*/
/*    font-weight: bold;*/
/*    color: #2c3e50;*/
/*    margin-bottom: 1rem;*/
/*}*/

/*.imagePreview, .pdfPreview {*/
/*    max-width: 100%;*/
/*    border-radius: 8px;*/
/*    margin-bottom: 1rem;*/
/*}*/

/*.documentLink {*/
/*    color: #3498db;*/
/*    text-decoration: none;*/
/*    font-weight: bold;*/
/*    border: 2px solid #3498db;*/
/*    padding: 0.5rem 1rem;*/
/*    border-radius: 8px;*/
/*    transition: background-color 0.3s ease, color 0.3s ease;*/
/*}*/

/*.documentLink:hover {*/
/*    background-color: #3498db;*/
/*    color: #fff;*/
/*}*/

/*.noFile {*/
/*    color: #888;*/
/*    font-size: 1.2rem;*/
/*    text-align: center;*/
/*    margin-top: 2rem;*/
/*}*/

/*.formsContainer {*/
/*    margin-top: 3rem;*/
/*    display: grid;*/
/*    grid-template-columns: 1fr 1fr;*/
/*    gap: 2rem;*/
/*}*/

/*.formsContainer > div {*/
    /*background-color: #fff;*/
/*    padding: 1.5rem;*/
    /*border-radius: 12px;*/
/*    box-shadow: 0 6px 16px rgba(0, 0, 0, 0.1);*/
/*    transition: transform 0.3s ease;*/
/*}*/

/*.sidebar {*/
/*    flex: 1;*/
/*    background: rgba(255, 255, 255, 0.9);*/
/*    padding: 20px;*/
/*    border-radius: 12px;*/
/*    margin-left: 20px;*/
/*    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);*/
/*}*/

/*.sidebarTitle {*/
/*    font-size: 1.6rem;*/
/*    font-weight: bold;*/
/*    margin-bottom: 1rem;*/
/*}*/

/*.formsContainer > div:hover {*/
/*    transform: translateY(-8px);*/
/*}*/

/*@media (max-width: 768px) {*/
/*    .formsContainer {*/
/*        grid-template-columns: 1fr;*/
/*    }*/

/*    .uploadDocuments {*/
/*        width: 100%;*/
/*    }*/
/*}*/


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

.noFile {
    color: #666;
    font-style: italic;
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

.formDisplay {
    flex: 1; /* Center form section */
    display: flex;
    justify-content: center;
    align-items: flex-start; /* Align to the top */
    background: rgba(0, 0, 0, 0.5);
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.37);
}
