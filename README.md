import React, { memo, useState, useEffect } from 'react';
import styles from './DocumentPreview.module.css';

const DocumentPreview = memo(({ document, index }) => {
    const [docURL, setDocURL] = useState(null);

    // Cache the document URL to prevent unnecessary re-renders
    useEffect(() => {
        if (document.url) {
            setDocURL(document.url);
        } else {
            setDocURL(URL.createObjectURL(document));
        }
        
        // Cleanup the URL object when the component unmounts
        return () => {
            if (!document.url) {
                URL.revokeObjectURL(docURL);
            }
        };
    }, [document, docURL]);

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
            
            {getDocumentType(document) === 'Image' ? (
                <img
                    src={docURL}
                    alt={document.name || "Document Preview"}
                    className={styles.imagePreview}
                />
            ) : getDocumentType(document) === 'PDF' ? (
                <iframe
                    src={`${docURL}#toolbar=0&navpanes=0&scrollbar=0`}
                    title={`PDF Preview ${index}`}
                    className={styles.pdfPreview}
                    frameBorder="0"
                />
            ) : (
                <a
                    href={docURL}
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
});

export default DocumentPreview;
