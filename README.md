const Sidebar = ({ onFileChange, onUpload, uploading, message }) => {
  const [selectedFile, setSelectedFile] = useState(null);

  const onDrop = useCallback((acceptedFiles) => {
    if (acceptedFiles && acceptedFiles.length > 0) {
      const file = acceptedFiles[0];
      console.log("File dropped:", file); // Debugging
      setSelectedFile(file);
      onFileChange(file);
    } else {
      console.error("No valid files dropped.");
    }
  }, [onFileChange]);

  const handleUpload = () => {
    console.log("Selected File before upload:", selectedFile); // Debugging
    if (selectedFile) {
      onUpload(selectedFile);
    } else {
      console.error("No file selected for upload.");
    }
  };

  const { getRootProps, getInputProps } = useDropzone({
    onDrop,
    multiple: false,
    accept: ".pdf, .xlsx, .xls, .csv, .docx",
  });

  return (
    <div className={styles.sidebar}>
      <h2 className={styles.heading}>Upload Product Sheet</h2>

      <div {...getRootProps()} className={styles.dropzoneContainer}>
        <input {...getInputProps()} />
        <div className={styles.dropzone}>
          <p>Drag & Drop your file here, or click to select</p>
        </div>
      </div>

      {selectedFile && (
        <div className={styles.fileNameContainer}>
          <p className={styles.fileName}>{selectedFile.name}</p>
        </div>
      )}

      <div className={styles.fileInputContainer}>
        {message && <p className={styles.message}>{message}</p>}
      </div>

      <button
        className={styles.uploadButton}
        onClick={handleUpload}
        disabled={uploading || !selectedFile}
      >
        {uploading ? (
          <div className={styles.loader}></div>
        ) : (
          <>
            <FontAwesomeIcon className={styles.icon} icon={faUpload} />
            Upload
          </>
        )}
      </button>
    </div>
  );
};
