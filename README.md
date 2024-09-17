import React from 'react';
import { useLocation } from 'react-router-dom';
import styles from './UploadDocuments.module.css';

const UploadDocuments = () => {
    const location = useLocation();
    const { uploadedFile } = location.state || {};

    return (
        <div className={styles.uploadDocuments}>
            <h2>Upload Documents - Review</h2>
            <div className={styles.reviewSection}>
                {uploadedFile ? (
                    <div className={styles.reviewItem}>
                        <strong>Uploaded Document:</strong> {uploadedFile.name}
                        <div className={styles.preview}>
                            {/* Show image preview if it's an image */}
                            {uploadedFile.type.startsWith('image/') && (
                                <img
                                    src={URL.createObjectURL(uploadedFile)}
                                    alt="Document Preview"
                                    className={styles.imagePreview}
                                />
                            )}
                            {/* Show PDF preview if it's a PDF */}
                            {uploadedFile.type === 'application/pdf' && (
                                <iframe
                                    src={URL.createObjectURL(uploadedFile)}
                                    title="PDF Preview"
                                    className={styles.pdfPreview}
                                />
                            )}
                            {/* Provide a link for other document types like DOCX */}
                            {uploadedFile.type === 'application/vnd.openxmlformats-officedocument.wordprocessingml.document' && (
                                <a
                                    href={URL.createObjectURL(uploadedFile)}
                                    download={uploadedFile.name}
                                    target="_blank"
                                    rel="noopener noreferrer"
                                    className={styles.documentLink}
                                >
                                    View Document (DOCX)
                                </a>
                            )}
                            {/* Fallback link for other file types */}
                            {!uploadedFile.type.startsWith('image/') &&
                                uploadedFile.type !== 'application/pdf' &&
                                uploadedFile.type !==
                                    'application/vnd.openxmlformats-officedocument.wordprocessingml.document' && (
                                <a
                                    href={URL.createObjectURL(uploadedFile)}
                                    download={uploadedFile.name}
                                    target="_blank"
                                    rel="noopener noreferrer"
                                    className={styles.documentLink}
                                >
                                    View Document
                                </a>
                            )}
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
