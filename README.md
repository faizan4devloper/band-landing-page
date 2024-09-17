import React, { useState, useEffect } from 'react';
import { useLocation } from 'react-router-dom';
import { pdfjs, Document, Page } from 'react-pdf';
import styles from './UploadDocuments.module.css';

// Set the worker path for PDF.js
pdfjs.GlobalWorkerOptions.workerSrc = `//cdnjs.cloudflare.com/ajax/libs/pdf.js/${pdfjs.version}/pdf.worker.min.js`;

const UploadDocuments = () => {
    const location = useLocation();
    const { uploadedFile } = location.state || {};
    const [pdfPreview, setPdfPreview] = useState(null);

    useEffect(() => {
        if (uploadedFile && uploadedFile.type === 'application/pdf') {
            // Convert PDF to image preview
            const fileUrl = URL.createObjectURL(uploadedFile);
            const pdf = pdfjs.getDocument(fileUrl);

            pdf.promise.then((pdfDoc) => {
                pdfDoc.getPage(1).then((page) => {
                    const canvas = document.createElement('canvas');
                    const context = canvas.getContext('2d');
                    const viewport = page.getViewport({ scale: 1 });
                    canvas.width = viewport.width;
                    canvas.height = viewport.height;

                    page.render({ canvasContext: context, viewport }).promise.then(() => {
                        setPdfPreview(canvas.toDataURL());
                    });
                });
            });

            return () => URL.revokeObjectURL(fileUrl);
        }
    }, [uploadedFile]);

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
                            {/* Show PDF preview as an image if it's a PDF */}
                            {uploadedFile.type === 'application/pdf' && pdfPreview && (
                                <img
                                    src={pdfPreview}
                                    alt="PDF Preview"
                                    className={styles.pdfPreview}
                                />
                            )}
                            {/* Provide a link for DOCX and other document types */}
                            {(uploadedFile.type === 'application/vnd.openxmlformats-officedocument.wordprocessingml.document' || !uploadedFile.type.startsWith('image/') && uploadedFile.type !== 'application/pdf') && (
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
