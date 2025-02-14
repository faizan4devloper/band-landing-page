useEffect(() => {
  if (data) {
    try {
      setDocumentName(
        data.CLAIM_FORM_DETAILS?.DOCUMENT_NAME || 
        data.UPLOADED_DOCUMENT_NAME || 
        uploadedFileName || 
        'Unnamed Document'
      );

      setClassification(data.CLAIM_FORM_DETAILS?.CLAIM_FORM_TYPE || 'Unknown');

      // Check if DETAILS_OF_ILLNESS exists and has content
      const hasIllnessDetails = data.DETAILS_OF_ILLNESS && Object.keys(data.DETAILS_OF_ILLNESS).length > 0;
      setIsValid(hasIllnessDetails ? 'Yes' : 'No');

    } catch (error) {
      console.error('Error processing classification data:', error);
      setDocumentName('Error Loading Document');
      setClassification('Error');
      setIsValid('Error');
    }
  } else {
    setDocumentName('');
    setClassification('');
    setIsValid('');
  }
}, [data, uploadedFileName]);
