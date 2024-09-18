
import React from 'react';
import { useLocation } from 'react-router-dom';
import styles from './UploadDocuments.module.css';

const UploadDocuments = () => {
    const location = useLocation();
    const { uploadedFile, documents } = location.state || {};

    return (
        <div className={styles.uploadDocuments}>
            <h2>Upload Documents - Review</h2>
            <div className={styles.reviewSection}>
                <div className={styles.reviewItem}>
                    <strong>Uploaded Document:</strong> 
                    {uploadedFile ? (
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
                                    width="500px"
                                    height="500px"
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
                    ) : (
                        <p className={styles.noFile}>No document uploaded</p>
                    )}
                </div>

                {/* Existing Documents Preview */}
                {documents && documents.length > 0 && (
                    <div className={styles.reviewItem}>
                        <strong>Existing Documents:</strong>
                        <div className={styles.preview}>
                            {documents.map((doc, index) => (
                                <div key={index} className={styles.previewItem}>
                                    {/* Show image preview if it's an image */}
                                    {doc.url.endsWith('.jpg') || doc.url.endsWith('.png') ? (
                                        <img
                                            src={doc.url}
                                            alt={doc.name}
                                            className={styles.imagePreview}
                                        />
                                    ) : doc.url.endsWith('.pdf') ? (
                                        <iframe
                                            src={doc.url}
                                            title={`PDF Preview ${index}`}
                                            className={styles.pdfPreview}
                                            width="500px"
                                            height="500px"
                                        />
                                    ) : (
                                        <a
                                            href={doc.url}
                                            download={doc.name}
                                            target="_blank"
                                            rel="noopener noreferrer"
                                            className={styles.documentLink}
                                        >
                                            {doc.name}
                                        </a>
                                    )}
                                </div>
                            ))}
                        </div>
                    </div>
                )}
            </div>
        </div>
    );
};

export default UploadDocuments;
