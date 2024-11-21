const handleUpload = async () => {
  if (!file) {
    setMessage("Please select a file to upload.");
    return;
  }
  setUploading(true);
  try {
    // Step 1: Request the presigned URL from the backend (dummy API)
    const response = await axios.post("dummy1", {
      payload: { filename: file.name },
    });
    
    console.log(response.data);

    const { presignedUrl, key } = response.data;

    // Step 2: Use the presigned URL to upload the file
    await axios.put(presignedUrl, file, {
      headers: { "Content-Type": file.type },
    });

    // Step 3: Call the new API to process the uploaded file and get claimId
    const newApiResponse = await axios.post("newApiEndpoint", {
      sqsKey: key, // Send the SQS key
      recNum: file.name, // Use the original filename for s3FileName
    });

    console.log("New API Response:", newApiResponse.data);

    const { claimId, s3FileName } = newApiResponse.data;

    // Step 4: Update the rows with the new information
    const newRecNum = rows.length + 1;
    setRows((prevRows) => [
      ...prevRows,
      {
        recNum: newRecNum,
        policyid: "",
        type: "",
        summary: "",
        previewLink: `https://your-s3-bucket-url.com/${s3FileName}`,
        status: "Pending",
        claimId, // Add claimId for future reference
      },
    ]);

    setMessage("File uploaded and processed successfully!");
    setIsUploaded(true); // Set flag to true after upload
  } catch (error) {
    setMessage("Upload failed: " + error.message);
    console.error("Error during upload:", error);
  } finally {
    setUploading(false);
  }
};
