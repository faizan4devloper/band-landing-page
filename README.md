ERROR in ./src/components/Pages/UploadDocuments.js 9:0-43
Module not found: Error: Can't resolve 'pdfjs-dist/build/pdf.worker.entry' in '/home/ec2-user/environment/Claim-assist/web-app/src/components/Pages'

import React, { useEffect, useRef } from 'react';
import { useLocation } from 'react-router-dom';
import styles from './UploadDocuments.module.css';
import * as pdfjsLib from 'pdfjs-dist';

// Set the worker source for PDF.js
pdfjsLib.GlobalWorkerOptions.workerSrc = `//cdnjs.cloudflare.com/ajax/libs/pdf.js/${pdfjsLib.version}/pdf.worker.js`;

const UploadDocuments = () => {
    const location = useLocation();
    const { uploadedFile, documents = [] } = location.state || {};

    // Combine uploaded file and existing documents into a single array
    const allDocuments = [
        ...(uploadedFile ? [uploadedFile] : []),
        ...documents
    ];

    const canvasRef = useRef(null);

    // Function to render a PDF onto a canvas
    const renderPDF = (pdfURL) => {
        const canvas = canvasRef.current;
        const context = canvas.getContext('2d');

        // Load the PDF document
        pdfjsLib.getDocument(pdfURL).promise.then((pdf) => {
            // Get the first page
            pdf.getPage(1).then((page) => {
                const viewport = page.getViewport({ scale: 1.5 });
                canvas.height = viewport.height;
                canvas.width = viewport.width;

                // Render the page on the canvas
                const renderContext = {
                    canvasContext: context,
                    viewport: viewport
                };
                page.render(renderContext);
            });
        });
    };

    useEffect(() => {
        if (uploadedFile && uploadedFile.type === 'application/pdf') {
            renderPDF(URL.createObjectURL(uploadedFile));
        } else if (documents.length > 0 && documents[0].url.endsWith('.pdf')) {
            renderPDF(documents[0].url);
        }
    }, [uploadedFile, documents]);

    return (
        <div className={styles.uploadDocuments}>
            <h2>Documents Review</h2>
            {allDocuments.length > 0 ? (
                <div className={styles.reviewSection}>
                    <div className={styles.preview}>
                        {allDocuments.map((doc, index) => (
                            <div key={index} className={styles.previewItem}>
                                {doc.type?.startsWith('image/') || doc.url?.endsWith('.jpg') || doc.url?.endsWith('.png') ? (
                                    <img
                                        src={doc.url ? doc.url : URL.createObjectURL(doc)}
                                        alt={doc.name || "Document Preview"}
                                        className={styles.imagePreview}
                                    />
                                ) : (doc.type === 'application/pdf' || doc.url?.endsWith('.pdf')) ? (
                                    // Render PDF onto a canvas
                                    <canvas ref={canvasRef} className={styles.pdfCanvas} />
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
        </div>
    );
};

export default UploadDocuments;
