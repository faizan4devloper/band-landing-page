function FileUploadSection({ 
  selectedFiles, 
  onFileChange, 
  onUpload,
  allowedFileTypes = ['.pdf', '.doc', '.docx'],
  uploadProgress = 0,
  uploadComplete = false,
}) {
  const [fileError, setFileError] = useState(null);
  const fileInputRef = useRef(null);

  const handleFileSelect = (event) => {
    const fileList = event.target.files;
    setFileError(null);

    if (fileList.length === 0) {
      setFileError('No file selected');
      return;
    }

    const validFiles = Array.from(fileList).filter((file) => {
      const fileExtension = '.' + file.name.split('.').pop().toLowerCase();
      return allowedFileTypes.includes(fileExtension);
    });

    if (validFiles.length === 0) {
      setFileError(`Invalid file type. Allowed types: ${allowedFileTypes.join(', ')}`);
      return;
    }

    onFileChange(validFiles); // Pass all valid files
  };

  const clearSelectedFiles = () => {
    onFileChange(null);
    if (fileInputRef.current) {
      fileInputRef.current.value = '';
    }
  };

  return (
    <div className={styles.fileUploadContainer}>
      <div className={styles.fileUploadSection}>
        <input 
          type="file" 
          multiple
          ref={fileInputRef}
          onChange={handleFileSelect}
          accept={allowedFileTypes.join(',')}
          style={{ display: 'none' }}
        />
        <button 
          onClick={() => fileInputRef.current?.click()}
          className={styles.fileSelectBtn}
          disabled={uploadProgress > 0 && uploadProgress < 100}
        >
          Select Files
        </button>
        {selectedFiles && selectedFiles.length > 0 && (
          <div className={styles.selectedFilesInfo}>
            {selectedFiles.map((file, index) => (
              <div key={index} className={styles.fileInfo}>
                <span>{file.name}</span>
                <span>{(file.size / 1024).toFixed(2)} KB</span>
              </div>
            ))}
            <button onClick={clearSelectedFiles} disabled={uploadProgress > 0 && uploadProgress < 100}>
              Clear Files
            </button>
          </div>
        )}
        <button 
          onClick={onUpload}
          disabled={!selectedFiles || selectedFiles.length === 0 || (uploadProgress > 0 && uploadProgress < 100)}
        >
          Upload
        </button>
      </div>
      {fileError && <div className={styles.fileErrorMessage}>{fileError}</div>}
    </div>
  );
}




<FileUploadSection 
  selectedFiles={selectedFiles} // Pass all files
  onFileChange={handleFileChange}
  onUpload={handleUpload}
  uploadProgress={uploadProgress}
  uploadComplete={uploadProgress === 100}
/>
