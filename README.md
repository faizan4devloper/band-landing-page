import React from 'react';
import { useLocation } from 'react-router-dom';
import styles from './UploadDocuments.module.css';

const UploadDocuments = () => {
    const location = useLocation();
    const {  uploadedFile } = location.state || {};

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
                                <img src={URL.createObjectURL(uploadedFile)} alt="Document Preview" />
                            )}
                            {/* Show a link for other document types */}
                            {!uploadedFile.type.startsWith('image/') && (
                                <a
                                    href={URL.createObjectURL(uploadedFile)}
                                    download={uploadedFile.name}
                                    target="_blank"
                                    rel="noopener noreferrer"
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
