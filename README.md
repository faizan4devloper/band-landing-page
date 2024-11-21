
const handleUpload = async () => {
  if (!file) {
    setMessage("Please select a file to upload.");
    return;
  }
  setUploading(true);
  try {
    // Step 1: Request the presigned URL from the dummy API
    const response = await axios.post("dummy1", {
      payload: { filename: file.name },
    });

    console.log("Dummy API Response:", response.data);

    const { presignedUrl, key } = response.data;

    // Step 2: Use the presigned URL to upload the file
    await axios.put(presignedUrl, file, {
      headers: { "Content-Type": file.type },
    });

    // Step 3: Prepare and send the payload to the new API
    const newApiPayload = {
      parameter1: key, // Example: SQS key or file key
      parameter2: file.name, // Example: Original filename
      parameter3: "additional-info", // Example: Add any static or dynamic info
    };

    const newApiResponse = await axios.post("newApiEndpoint", newApiPayload, {
      headers: { "Content-Type": "application/json" },
    });

    console.log("New API Response:", newApiResponse.data);

    const { claimId, s3FileName, additionalData } = newApiResponse.data;

    // Step 4: Update the rows with the new information
    const newRecNum = rows.length + 1;
    setRows((prevRows) => [
      ...prevRows,
      {
        recNum: newRecNum,
        policyid: "",
        type: "",
        summary: additionalData, // Example: Use additional data if provided
        previewLink: `https://your-s3-bucket-url.com/${s3FileName}`,
        status: "Pending",
        claimId, // Store the claimId for future use
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
