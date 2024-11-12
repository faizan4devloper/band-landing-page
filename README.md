// Modal component for preview
const FilePreviewModal = ({ file, closeModal }) => {
  const [fileContent, setFileContent] = useState(null);

  const readFile = (file) => {
    const reader = new FileReader();

    if (file.type.startsWith('text')) {
      reader.onload = () => {
        setFileContent(reader.result);
      };
      reader.readAsText(file);
    } else if (file.type.startsWith('image')) {
      setFileContent(URL.createObjectURL(file));
    } else if (file.type === 'application/pdf') {
      setFileContent(URL.createObjectURL(file)); // Set URL for PDF preview
    } else {
      setFileContent('Unable to preview this file type');
    }
  };

  React.useEffect(() => {
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
          <iframe
            src={fileContent}
            title="PDF Preview"
            className={styles.pdfPreview}
          />
        )}
        {file && !file.type.startsWith('text') && !file.type.startsWith('image') && file.type !== 'application/pdf' && (
          <p>{fileContent}</p>
        )}

        <button onClick={closeModal} className={styles.closeButton}>Close</button>
      </div>
    </div>
  );
};





.pdfPreview {
  width: 100%;
  height: 500px;
  border: none;
}
