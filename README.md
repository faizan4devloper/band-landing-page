const [uploadedFileName, setUploadedFileName] = useState(""); 

const handleFileChange = (e) => {
  const selectedFile = e.target.files[0];
  if (selectedFile) {
    const sanitizedFileName = selectedFile.name.replace(/\s+/g, "_");
    setFile(selectedFile);
    setUploadedFileName(sanitizedFileName); // Store the file name
    setPreviewUrl(URL.createObjectURL(selectedFile)); 
    setMessage("");
  }
};







{isUploaded ? (
  <MainContent 
    message={message} 
    selectedPolicy={selectedPolicy}
    rows={rows} 
    clid={rows[0].recNum}
    setRows={setRows} 
    staticPreviewUrl={previewUrl} 
    uploadedFileName={uploadedFileName} // Pass file name
  />
) : (
  <p className={styles.infoMessage}>
    Please upload a document to view the data.
  </p>
)}







const ClaimClassification = ({ data, uploadedFileName }) => {
  const [documentName, setDocumentName] = useState('');

  useEffect(() => {
    if (uploadedFileName) {
      setDocumentName(uploadedFileName);
    } else if (data && data.total_extracted_data) {
      try {
        const parsedData = JSON.parse(data.total_extracted_data);
        setDocumentName(
          parsedData.CLAIM_FORM_DETAILS?.DOCUMENT_NAME || 
          parsedData.UPLOADED_DOCUMENT_NAME || 
          'Unnamed Document'
        );
      } catch (error) {
        console.error('Error parsing extracted data:', error);
        setDocumentName('Unknown Document');
      }
    }
  }, [data, uploadedFileName]);

  return (
    <table className={styles.classificationTable}>
      <thead>
        <tr>
          <th>Document Name</th>
          <th>Classification</th>
          <th>Is Valid?</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>{documentName}</td>
          <td> {/* Other classification details here */} </td>
        </tr>
      </tbody>
    </table>
  );
};
