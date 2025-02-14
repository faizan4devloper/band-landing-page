const ClaimClassification = ({ data, uploadedFileName }) => {


useEffect(() => {
  if (data) {
    try {
      setDocumentName(
        data.CLAIM_FORM_DETAILS?.DOCUMENT_NAME || 
        data.UPLOADED_DOCUMENT_NAME || 
        uploadedFileName || // Add this line
        'Unnamed Document'
      );
      // ... rest of your useEffect logic
    }
  }
}, [data, uploadedFileName]); // Add uploadedFileName to dependencies



<ClaimClassification
  data={data?.total_extracted_data ? JSON.parse(data.total_extracted_data) : null}
  uploadedFileName={uploadedFileName} // This is correctly passed
/>
