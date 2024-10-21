import React, { useState, useRef, useEffect } from 'react';
import styles from './DocumentPreview.module.css';

const DocumentPreview = ({ document, index }) => {
    const iframeRef = useRef(null);
    const [docUrl, setDocUrl] = useState('');

    useEffect(() => {
        if (document.url) {
            setDocUrl(document.url);
        } else {
            const objectUrl = URL.createObjectURL(document);
            setDocUrl(objectUrl);

            // Clean up the object URL to avoid memory leaks
            return () => URL.revokeObjectURL(objectUrl);
        }
    }, [document]);

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

            {getDocumentType(document) === 'PDF' ? (
                <iframe
                    ref={iframeRef}
                    src={`${docUrl}#toolbar=0&navpanes=0&scrollbar=0`}
                    title={`PDF Preview ${index}`}
                    className={styles.pdfPreview}
                    width="400px"
                    height="550px"
                    frameBorder="0"
                />
            ) : getDocumentType(document) === 'Image' ? (
                <img
                    src={docUrl}
                    alt={document.name || "Document Preview"}
                    className={styles.imagePreview}
                />
            ) : (
                <a
                    href={docUrl}
                    download={document.name || "Document"}
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




.previewItem {
    /*margin-bottom: 20px;*/
    padding: 10px;
    border: 1px solid #e0e0e0;
    background: #ffffff;
    padding-bottom: 0;
    max-width: 100%; /* Ensure it does not exceed the parent width */
    overflow: hidden; /* Prevent overflow */
    
}

.imagePreview {
    max-width: 100%;
    max-height: 200px; /* Limit the height for images */
    object-fit: contain; /* Maintain aspect ratio */
    border-radius: 8px; /* Optional: adds a nicer look */
}

.pdfPreview {
    width: 100%; /* Full width of the parent */
    height: 400px; /* Limit the height for PDFs */
}

.documentLink {
    text-decoration: none;
    color: #007bff;
    font-weight: bold;
    display: block; /* Make it block level to fill available width */
    text-align: center; /* Center text */
}

.documentType {
    text-align: center;
    margin: 0px;
}
