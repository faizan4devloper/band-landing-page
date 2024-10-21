// DocumentPreview.js
import React from 'react';
import styles from './DocumentPreview.module.css';

const DocumentPreview = ({ document, index }) => {
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
            {document.type?.startsWith('image/') || document.url?.endsWith('.jpg') || document.url?.endsWith('.png') ? (
                <img
                    src={document.url ? document.url : URL.createObjectURL(document)}
                    alt={document.name || "Document Preview"}
                    className={styles.imagePreview}
                />
            ) : document.type === 'application/pdf' || document.url?.endsWith('.pdf') ? (
                <iframe
                    src={`${document.url ? document.url : URL.createObjectURL(document)}#toolbar=0&navpanes=0&scrollbar=0`}
                    title={`PDF Preview ${index}`}
                    className={styles.pdfPreview}
                    frameBorder="0"
                />
            ) : (
                <a
                    href={document.url ? document.url : URL.createObjectURL(document)}
                    download={document.name || document.name}
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

