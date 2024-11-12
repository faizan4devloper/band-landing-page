import React, { useState, useEffect } from 'react';
import * as pdfjsLib from 'pdfjs-dist';
import styles from './FilePreviewModal.module.css';

// PDF worker
pdfjsLib.GlobalWorkerOptions.workerSrc = `//cdnjs.cloudflare.com/ajax/libs/pdf.js/${pdfjsLib.version}/pdf.worker.js`;

const FilePreviewModal = ({ file, closeModal }) => {
  const [fileContent, setFileContent] = useState(null);
  const [pdfPages, setPdfPages] = useState([]);

  const renderPdf = async (file) => {
    const arrayBuffer = await file.arrayBuffer();
    const pdf = await pdfjsLib.getDocument(arrayBuffer).promise;
    const pages = [];

    for (let i = 1; i <= pdf.numPages; i++) {
      const page = await pdf.getPage(i);
      const viewport = page.getViewport({ scale: 1.5 });
      const canvas = document.createElement('canvas');
      const context = canvas.getContext('2d');
      canvas.width = viewport.width;
      canvas.height = viewport.height;

      await page.render({ canvasContext: context, viewport }).promise;
      pages.push(canvas.toDataURL());
    }
    setPdfPages(pages);
  };

  const readFile = (file) => {
    if (file.type.startsWith('text')) {
      const reader = new FileReader();
      reader.onload = () => setFileContent(reader.result);
      reader.readAsText(file);
    } else if (file.type.startsWith('image')) {
      setFileContent(URL.createObjectURL(file));
    } else if (file.type === 'application/pdf') {
      renderPdf(file);
    } else {
      setFileContent('Unable to preview this file type');
    }
  };

  useEffect(() => {
    if (file) {
      readFile(file);
    }
  }, [file]);

  return (
    <div className={styles.modalOverlay} onClick={closeModal}>
      <div className={styles.modalContent} onClick={(e) => e.stopPropagation()}>
        <h2>File Preview</h2>

        {file && file.type.startsWith('text') && <pre>{fileContent}</pre>}
        {file && file.type.startsWith('image') && (
          <img src={fileContent} alt="preview" className={styles.imagePreview} />
        )}
        {file && file.type === 'application/pdf' && pdfPages.length > 0 && (
          <div className={styles.pdfPages}>
            {pdfPages.map((page, index) => (
              <img key={index} src={page} alt={`PDF page ${index + 1}`} className={styles.pdfImage} />
            ))}
          </div>
        )}
        {file && !file.type.startsWith('text') && !file.type.startsWith('image') && file.type !== 'application/pdf' && (
          <p>{fileContent}</p>
        )}

        <button onClick={closeModal} className={styles.closeButton}>Close</button>
      </div>
    </div>
  );
};

export default FilePreviewModal;




/* CSS for PDF image preview */
.pdfPages {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin-top: 20px;
}

.pdfImage {
  width: 100%;
  max-width: 500px;
  height: auto;
}
