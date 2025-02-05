useEffect(() => {
  console.log("Received Data:", data);
  
  if (uploadedFileName) {
    setDocumentName(uploadedFileName);
  } else if (data && data.total_extracted_data) {
    console.log("Raw Extracted Data:", data.total_extracted_data);
    
    try {
      const parsedData = JSON.parse(data.total_extracted_data);
      console.log("Parsed Data:", parsedData);

      setDocumentName(
        parsedData.CLAIM_FORM_DETAILS?.DOCUMENT_NAME || 
        parsedData.UPLOADED_DOCUMENT_NAME || 
        'Unnamed Document'
      );

      const formType = parsedData.CLAIM_FORM_DETAILS?.CLAIM_FORM_TYPE || 'Unknown';
      console.log("Extracted Classification:", formType);
      setClassification(formType);

      const cardiomyopathy = parsedData.DETAILS_OF_ILLNESS?.HEART_SURGERY_DETAILS?.PATIENT_SUFFERED_FROM_CARDIOMIOPATHY || 'N/A';
      console.log("Extracted Cardiomyopathy:", cardiomyopathy);

      const validationStatus = cardiomyopathy.toLowerCase() === 'no' ? 'Yes' : 'No';
      console.log("Extracted Validity:", validationStatus);
      setIsValid(validationStatus);

    } catch (error) {
      console.error('Error parsing extracted data:', error);
      setDocumentName('Unknown Document');
      setClassification('Unknown');
      setIsValid('Error');
    }
  }
}, [data, uploadedFileName]);
