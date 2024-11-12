ERROR in ./src/components/Pages/Job/FilePreviewModal.js 6:0-35
Module not found: Error: Can't resolve 'react-pdf' in '/home/ec2-user/environment/citizen-advisor-v2/web-app/src/components/Pages/Job'
ERROR in ./src/components/Pages/Job/FilePreviewModal.js 11:38-100
Module not found: Error: Can't resolve 'pdfjs-dist/build/pdf.worker.min.js' in '/home/ec2-user/environment/citizen-advisor-v2/web-app/src/components/Pages/Job'




import React, { useState, useEffect } from 'react';
import * as pdfjs from 'react-pdf';
import styles from './FilePreviewModal.module.css';

// PDF worker setup

pdfjs.GlobalWorkerOptions.workerSrc = new URL('pdfjs-dist/build/pdf.worker.min.js', import.meta.url).toString();

const FilePreviewModal = ({ file, closeModal }) => {
  const [fileContent, setFileContent] = useState(null);
  const [pdfTextContent, setPdfTextContent] = useState('');

  // Function to extract text content from PDF
  const renderPdfText = async (file) => {
    const arrayBuffer = await file.arrayBuffer();
    const pdf = await pdfjs.getDocument(arrayBuffer).promise;
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
