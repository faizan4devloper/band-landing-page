{
    "category": "Payment Instruction Form",
    "extracted_data": {
        "PAYMENT_INSTRUCTION_FORM": {
            "STATEMENT_DATE": "12/06/2024",
            "POLICY_NUMBER": "L2065777",
            "POLICY_ON_THE_LIFE_OF": "Mr JC Mcglynn",
            "POLICY_OWNER": "Mr JC Mcglynn"
        },
        "PAYMENT_DETAILS": {
            "BANK_NAME_AND_ADDRESS": "BARCLAYS BANK",
            "ACCOUNT_HOLDERS_NAME": "J.C.MC Ghywon",
            "ACCOUNT_NUMBER": "50614866",
            "BANK_SORT_CODE": "20-57-40",
            "SIGNED_FULL_NAME": "Mr Mcglynn, James Christopher",
            "SIGNED_DATE": "13/06/2024"
        },
        "LOST_POLICY_FORM": {
            "STATEMENT_DATE": "12/06/2024",
            "POLICY_NUMBER": "L2065777",
            "POLICY_ON_THE_LIFE_OF": "Mr J C Mcglynn",
            "POLICY_OWNER": "Mr JC Mcglynn"
        },
        "LOST_POLICY_FORM_SIGNED": {
            "FULL_NAME": "Mr Mcglynn, James Christopher",
            "DATE": "13/06/2024"
        },
        "LOST_POLICY_FORM_WITNESSED_BY": {
            "FULL_NAME_OF_WITNESS": "SOPHIE PASSFIELD",
            "DATE": "13/06/2024",
            "ADDRESS_OF_WITNESS": "53 ORNE GARDENS, BOLBECIC PARK, MILTON KEYNES MK18 8PG",
            "OFFICIAL_STAMP": "",
            "DAY-TIME_TELEPHONE_NUMBER_OF_WITNESS": "07732883 700",
            "OCCUPATION_OF_WITNESS": "Teacher"
        }
    },
    "unfilledpercent": "4%"
}


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
            <PaymentInstructionForm formData={formData} />
            <PaymentDetails paymentData={paymentData} />
            <LostPolicyForm formData={formData} />
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

PaymentInstructionForm.js:8 Uncaught TypeError: Cannot read properties of undefined (reading 'POLICY_NUMBER')
    at PaymentInstructionForm (PaymentInstructionForm.js:8:1)
    at renderWithHooks (react-dom.development.js:15501:1)
    at mountIndeterminateComponent (react-dom.development.js:20118:1)
    at beginWork (react-dom.development.js:21640:1)
    at HTMLUnknownElement.callCallback (react-dom.development.js:4176:1)
    at Object.invokeGuardedCallbackDev (react-dom.development.js:4227:1)
    at invokeGuardedCallback (react-dom.development.js:4288:1)
    at beginWork$1 (react-dom.development.js:27505:1)
    at performUnitOfWork (react-dom.development.js:26610:1)
    at workLoopSync (react-dom.development.js:26518:1)
PaymentDetails.js:8 Uncaught TypeError: Cannot read properties of undefined (reading 'BANK_NAME_AND_ADDRESS')
    at PaymentDetails (PaymentDetails.js:8:1)
    at renderWithHooks (react-dom.development.js:15501:1)
    at mountIndeterminateComponent (react-dom.development.js:20118:1)
    at beginWork (react-dom.development.js:21640:1)
    at HTMLUnknownElement.callCallback (react-dom.development.js:4176:1)
    at Object.invokeGuardedCallbackDev (react-dom.development.js:4227:1)
    at invokeGuardedCallback (react-dom.development.js:4288:1)
    at beginWork$1 (react-dom.development.js:27505:1)
    at performUnitOfWork (react-dom.development.js:26610:1)
    at workLoopSync (react-dom.development.js:26518:1)
LostPolicyForm.js:8 Uncaught TypeError: Cannot read properties of undefined (reading 'STATEMENT_DATE')
    at LostPolicyForm (LostPolicyForm.js:8:1)
    at renderWithHooks (react-dom.development.js:15501:1)
    at mountIndeterminateComponent (react-dom.development.js:20118:1)
    at beginWork (react-dom.development.js:21640:1)
    at HTMLUnknownElement.callCallback (react-dom.development.js:4176:1)
    at Object.invokeGuardedCallbackDev (react-dom.development.js:4227:1)
    at invokeGuardedCallback (react-dom.development.js:4288:1)
    at beginWork$1 (react-dom.development.js:27505:1)
    at performUnitOfWork (react-dom.development.js:26610:1)
    at workLoopSync (react-dom.development.js:26518:1)
WitnessDetails.js:9 Uncaught TypeError: Cannot read properties of undefined (reading 'FULL_NAME_OF_WITNESS')
    at WitnessDetails (WitnessDetails.js:9:1)
    at renderWithHooks (react-dom.development.js:15501:1)
    at mountIndeterminateComponent (react-dom.development.js:20118:1)
    at beginWork (react-dom.development.js:21640:1)
    at HTMLUnknownElement.callCallback (react-dom.development.js:4176:1)
    at Object.invokeGuardedCallbackDev (react-dom.development.js:4227:1)
    at invokeGuardedCallback (react-dom.development.js:4288:1)
    at beginWork$1 (react-dom.development.js:27505:1)
    at performUnitOfWork (react-dom.development.js:26610:1)
    at workLoopSync (react-dom.development.js:26518:1)
PaymentInstructionForm.js:8 Uncaught TypeError: Cannot read properties of undefined (reading 'POLICY_NUMBER')
    at PaymentInstructionForm (PaymentInstructionForm.js:8:1)
    at renderWithHooks (react-dom.development.js:15501:1)
    at mountIndeterminateComponent (react-dom.development.js:20118:1)
    at beginWork (react-dom.development.js:21640:1)
    at HTMLUnknownElement.callCallback (react-dom.development.js:4176:1)
    at Object.invokeGuardedCallbackDev (react-dom.development.js:4227:1)
    at invokeGuardedCallback (react-dom.development.js:4288:1)
    at beginWork$1 (react-dom.development.js:27505:1)
    at performUnitOfWork (react-dom.development.js:26610:1)
    at workLoopSync (react-dom.development.js:26518:1)
PaymentDetails.js:8 Uncaught TypeError: Cannot read properties of undefined (reading 'BANK_NAME_AND_ADDRESS')
    at PaymentDetails (PaymentDetails.js:8:1)
    at renderWithHooks (react-dom.development.js:15501:1)
    at mountIndeterminateComponent (react-dom.development.js:20118:1)
    at beginWork (react-dom.development.js:21640:1)
    at HTMLUnknownElement.callCallback (react-dom.development.js:4176:1)
    at Object.invokeGuardedCallbackDev (react-dom.development.js:4227:1)
    at invokeGuardedCallback (react-dom.development.js:4288:1)
    at beginWork$1 (react-dom.development.js:27505:1)
    at performUnitOfWork (react-dom.development.js:26610:1)
    at workLoopSync (react-dom.development.js:26518:1)
LostPolicyForm.js:8 Uncaught TypeError: Cannot read properties of undefined (reading 'STATEMENT_DATE')
    at LostPolicyForm (LostPolicyForm.js:8:1)
    at renderWithHooks (react-dom.development.js:15501:1)
    at mountIndeterminateComponent (react-dom.development.js:20118:1)
    at beginWork (react-dom.development.js:21640:1)
    at HTMLUnknownElement.callCallback (react-dom.development.js:4176:1)
    at Object.invokeGuardedCallbackDev (react-dom.development.js:4227:1)
    at invokeGuardedCallback (react-dom.development.js:4288:1)
    at beginWork$1 (react-dom.development.js:27505:1)
    at performUnitOfWork (react-dom.development.js:26610:1)
    at workLoopSync (react-dom.development.js:26518:1)
WitnessDetails.js:9 Uncaught TypeError: Cannot read properties of undefined (reading 'FULL_NAME_OF_WITNESS')
    at WitnessDetails (WitnessDetails.js:9:1)
    at renderWithHooks (react-dom.development.js:15501:1)
    at mountIndeterminateComponent (react-dom.development.js:20118:1)
    at beginWork (react-dom.development.js:21640:1)
    at HTMLUnknownElement.callCallback (react-dom.development.js:4176:1)
    at Object.invokeGuardedCallbackDev (react-dom.development.js:4227:1)
    at invokeGuardedCallback (react-dom.development.js:4288:1)
    at beginWork$1 (react-dom.development.js:27505:1)
    at performUnitOfWork (react-dom.development.js:26610:1)
    at workLoopSync (react-dom.development.js:26518:1)
react-dom.development.js:18722 The above error occurred in the <PaymentInstructionForm> component:

    at PaymentInstructionForm (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/main.1a29f9ece7398c2c342a.hot-update.js:26:3)
    at div
    at UploadDocuments (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:1523:81)
    at RenderedRoute (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:47001:5)
    at Routes (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:47703:5)
    at div
    at Router (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:47637:15)
    at BrowserRouter (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:45578:5)
    at App (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:44:80)

Consider adding an error boundary to your tree to customize error handling behavior.
Visit https://reactjs.org/link/error-boundaries to learn more about error boundaries.
logCapturedError @ react-dom.development.js:18722
Show 1 more frame
Show less
react-dom.development.js:18722 The above error occurred in the <PaymentDetails> component:

    at PaymentDetails (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:603:3)
    at div
    at UploadDocuments (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:1523:81)
    at RenderedRoute (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:47001:5)
    at Routes (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:47703:5)
    at div
    at Router (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:47637:15)
    at BrowserRouter (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:45578:5)
    at App (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:44:80)

Consider adding an error boundary to your tree to customize error handling behavior.
Visit https://reactjs.org/link/error-boundaries to learn more about error boundaries.
logCapturedError @ react-dom.development.js:18722
Show 1 more frame
Show less
react-dom.development.js:18722 The above error occurred in the <LostPolicyForm> component:

    at LostPolicyForm (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:477:3)
    at div
    at UploadDocuments (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:1523:81)
    at RenderedRoute (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:47001:5)
    at Routes (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:47703:5)
    at div
    at Router (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:47637:15)
    at BrowserRouter (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:45578:5)
    at App (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:44:80)

Consider adding an error boundary to your tree to customize error handling behavior.
Visit https://reactjs.org/link/error-boundaries to learn more about error boundaries.
logCapturedError @ react-dom.development.js:18722
Show 1 more frame
Show less
react-dom.development.js:18722 The above error occurred in the <WitnessDetails> component:

    at WitnessDetails (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:879:3)
    at div
    at UploadDocuments (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:1523:81)
    at RenderedRoute (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:47001:5)
    at Routes (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:47703:5)
    at div
    at Router (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:47637:15)
    at BrowserRouter (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:45578:5)
    at App (https://a6adf01bb0a740879b83bbee309c7227.vfs.cloud9.us-east-1.amazonaws.com/static/js/bundle.js:44:80)

Consider adding an error boundary to your tree to customize error handling behavior.
Visit https://reactjs.org/link/error-boundaries to learn more about error boundaries.
logCapturedError @ react-dom.development.js:18722
Show 1 more frame
Show less
react-dom.development.js:26975 Uncaught TypeError: Cannot read properties of undefined (reading 'POLICY_NUMBER')
    at PaymentInstructionForm (PaymentInstructionForm.js:8:1)
    at renderWithHooks (react-dom.development.js:15501:1)
    at mountIndeterminateComponent (react-dom.development.js:20118:1)
    at beginWork (react-dom.development.js:21640:1)
    at beginWork$1 (react-dom.development.js:27478:1)
    at performUnitOfWork (react-dom.development.js:26610:1)
    at workLoopSync (react-dom.development.js:26518:1)
    at renderRootSync (react-dom.development.js:26487:1)
    at recoverFromConcurrentError (react-dom.development.js:25902:1)
    at performConcurrentWorkOnRoot (react-dom.development.js:25803:1)
