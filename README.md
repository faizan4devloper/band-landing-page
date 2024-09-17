import React from 'react';
import { useLocation } from 'react-router-dom';
import styles from './UploadDocuments.module.css';

const UploadDocuments = () => {
    const location = useLocation();
    const { uploadedFile, documents } = location.state || {};

    const renderDocumentPreview = (file) => {
        if (file.type.startsWith('image/')) {
            return <img src={URL.createObjectURL(file)} alt="Document Preview" className={styles.imagePreview} />;
        } else if (file.type === 'application/pdf') {
            return (
                <iframe
                    src={URL.createObjectURL(file)}
                    title="PDF Preview"
                    className={styles.pdfPreview}
                    width="600px"
                    height="600px"
                />
            );
        } else if (file.type === 'application/vnd.openxmlformats-officedocument.wordprocessingml.document') {
            return (
                <a
                    href={URL.createObjectURL(file)}
                    download={file.name}
                    target="_blank"
                    rel="noopener noreferrer"
                    className={styles.documentLink}
                >
                    View Document (DOCX)
                </a>
            );
        } else {
            return (
                <a
                    href={URL.createObjectURL(file)}
                    download={file.name}
                    target="_blank"
                    rel="noopener noreferrer"
                    className={styles.documentLink}
                >
                    View Document
                </a>
            );
        }
    };

    return (
        <div className={styles.uploadDocuments}>
            <h2>Upload Documents - Review</h2>
            <div className={styles.reviewSection}>
                {(uploadedFile || documents.length > 0) ? (
                    <div className={styles.reviewItem}>
                        <strong>Document Preview:</strong>
                        <div className={styles.preview}>
                            {uploadedFile && renderDocumentPreview(uploadedFile)}
                            {documents && documents.map((doc, index) => (
                                <div key={index}>
                                    <strong>Existing Document:</strong>
                                    {renderDocumentPreview({
                                        name: doc.name,
                                        type: doc.type,
                                        url: doc.url
                                    })}
                                </div>
                            ))}
                        </div>
                    </div>
                ) : (
                    <div className={styles.reviewItem}>
                        <strong>No Document Uploaded</strong>
                        <p className={styles.noFile}>Please upload a document to preview it here.</p>
                    </div>
                )}
            </div>
        </div>
    );
};

export default UploadDocuments;
