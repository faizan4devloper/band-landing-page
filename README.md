import React from 'react';
import { useLocation } from 'react-router-dom';
import styles from './UploadDocuments.module.css';

const UploadDocuments = () => {
    const location = useLocation();
    const { uploadedFile, documents } = location.state || {};

    const renderPreview = (file) => {
        if (!file) return null;

        if (file.type.startsWith('image/')) {
            return <img src={URL.createObjectURL(file)} alt="Document Preview" className={styles.imagePreview} />;
        }

        if (file.type === 'application/pdf') {
            return (
                <iframe
                    src={URL.createObjectURL(file)}
                    title="PDF Preview"
                    className={styles.pdfPreview}
                    width="600px"
                    height="600px"
                />
            );
        }

        if (file.type === 'application/vnd.openxmlformats-officedocument.wordprocessingml.document') {
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
        }

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
    };

    return (
        <div className={styles.uploadDocuments}>
            <h2>Document Review</h2>
            <div className={styles.reviewSection}>
                {/* Display uploaded document preview */}
                {uploadedFile && (
                    <div className={styles.reviewItem}>
                        <strong>Uploaded Document:</strong> {uploadedFile.name}
                        <div className={styles.preview}>
                            {renderPreview(uploadedFile)}
                        </div>
                    </div>
                )}

                {/* Display existing documents preview */}
                {documents && documents.length > 0 && (
                    <div className={styles.reviewItem}>
                        <strong>Existing Documents:</strong>
                        <div className={styles.preview}>
                            {documents.map((doc, index) => (
                                <div key={index}>
                                    <strong>{doc.name}</strong>: 
                                    <a href={doc.url} target="_blank" rel="noopener noreferrer">
                                        View Document
                                    </a>
                                </div>
                            ))}
                        </div>
                    </div>
                )}

                {/* Handle case where no documents are available */}
                {!uploadedFile && (!documents || documents.length === 0) && (
                    <div className={styles.reviewItem}>
                        <strong>No Document Available</strong>
                        <p className={styles.noFile}>Please upload a document or select an existing one to preview it here.</p>
                    </div>
                )}
            </div>
        </div>
    );
};

export default UploadDocuments;
