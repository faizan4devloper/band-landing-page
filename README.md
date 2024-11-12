import React, { useState, useEffect } from 'react';
import { GlobalWorkerOptions, getDocument } from 'pdfjs-dist';
import styles from './FilePreviewModal.module.css';

// PDF worker setup
GlobalWorkerOptions.workerSrc = `//cdnjs.cloudflare.com/ajax/libs/pdf.js/${pdfjs.version}/pdf.worker.min.js`;

const FilePreviewModal = ({ file, closeModal }) => {
  const [fileContent, setFileContent] = useState(null);
  const [pdfTextContent, setPdfTextContent] = useState('');

  // Function to extract text content from PDF
  const renderPdfText = async (file) => {
    const arrayBuffer = await file.arrayBuffer();
    const pdf = await getDocument(arrayBuffer).promise;
    let textContent = '';

    for (let i = 1; i <= pdf.numPages; i++) {
      const page = await pdf.getPage(i);
      const textContentItems = await page.getTextContent();

      textContentItems.items.forEach((item) => {
        textContent += item.str + ' ';
      });
      textContent += '\n\n'; // Separate pages with line breaks
    }

    setPdfTextContent(textContent);
  };

  const readFile = (file) => {
    if (file.type.startsWith('text')) {
      const reader = new FileReader();
      reader.onload = () => setFileContent(reader.result);
      reader.readAsText(file);
    } else if (file.type.startsWith('image')) {
      setFileContent(URL.createObjectURL(file));
    } else if (file.type === 'application/pdf') {
      renderPdfText(file);
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
        {file && file.type === 'application/pdf' && (
          <pre className={styles.pdfTextPreview}>{pdfTextContent}</pre>
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



.pdfTextPreview {
  white-space: pre-wrap;
  word-wrap: break-word;
  text-align: left;
  background-color: #f4f4f4;
  padding: 10px;
  border-radius: 5px;
  margin-top: 20px;
  max-height: 400px;
  overflow-y: auto;
}
