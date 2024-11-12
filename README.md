import React, { useState, useEffect } from 'react';
import * as pdfjsLib from 'pdfjs-dist';
import 'pdfjs-dist/build/pdf.worker.entry';
import styles from './Sidebar.module.css';

const FilePreviewModal = ({ file, closeModal }) => {
  const [pdfPages, setPdfPages] = useState([]);

  const loadPdf = async (file) => {
    try {
      const pdfData = await file.arrayBuffer(); // Convert file to ArrayBuffer
      const pdf = await pdfjsLib.getDocument(pdfData).promise; // Load PDF
      const pages = [];

      // Render each page as an image
      for (let pageNum = 1; pageNum <= pdf.numPages; pageNum++) {
        const page = await pdf.getPage(pageNum);
        const viewport = page.getViewport({ scale: 1.5 }); // Adjust scale as needed
        const canvas = document.createElement('canvas');
        const context = canvas.getContext('2d');
        canvas.height = viewport.height;
        canvas.width = viewport.width;

        await page.render({ canvasContext: context, viewport }).promise;
        pages.push(canvas.toDataURL('image/png')); // Store rendered page as an image
      }

      setPdfPages(pages);
    } catch (error) {
      console.error('Error loading PDF:', error);
    }
  };

  useEffect(() => {
    if (file && file.type === 'application/pdf') {
      loadPdf(file);
    }
  }, [file]);

  return (
    <div className={styles.modalOverlay} onClick={closeModal}>
      <div className={styles.modalContent} onClick={(e) => e.stopPropagation()}>
        <h2>PDF Preview</h2>
        <div className={styles.pdfContainer}>
          {pdfPages.length > 0 ? (
            pdfPages.map((pageSrc, index) => (
              <img
                key={index}
                src={pageSrc}
                alt={`PDF page ${index + 1}`}
                className={styles.pdfPageImage}
              />
            ))
          ) : (
            <p>Loading PDF...</p>
          )}
        </div>
        <button onClick={closeModal} className={styles.closeButton}>Close</button>
      </div>
    </div>
  );
};

export default FilePreviewModal;





.modalOverlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.6);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modalContent {
  background: white;
  padding: 20px;
  border-radius: 8px;
  width: 80%;
  max-height: 80%;
  overflow-y: auto;
  position: relative;
}

.pdfContainer {
  display: flex;
  flex-direction: column;
  gap: 10px;
  align-items: center;
}

.pdfPageImage {
  width: 100%;
  max-width: 600px; /* Adjust to your preference */
  margin: 0 auto;
  display: block;
}

.closeButton {
  margin-top: 20px;
  padding: 8px 16px;
  background-color: #0073e6;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.closeButton:hover {
  background-color: #005bb5;
}
