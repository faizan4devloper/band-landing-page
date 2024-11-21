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

    // Step 3: Prepare the payload for pushing to SQS
    const claimId = `CL${Math.floor(1000000 + Math.random() * 9000000)}`; // Generate a unique Claim ID
    const taskType = "SEND_TO_QUEUE";
    const s3FileName = key; // Use the returned S3 key as the filename

    const sqsPayload = {
      claimid: claimId,
      s3filename: s3FileName,
      tasktype: taskType,
    };

    // Step 4: Send the payload to the new API
    const sqsResponse = await axios.post("newApiEndpoint", sqsPayload, {
      headers: { "Content-Type": "application/json" },
    });

    console.log("SQS API Response:", sqsResponse.data);

    // Step 5: Update the rows with the new information
    const newRecNum = rows.length + 1;
    setRows((prevRows) => [
      ...prevRows,
      {
        recNum: newRecNum,
        policyid: "",
        type: "",
        summary: "", // No additional data in the example
        previewLink: `https://your-s3-bucket-url.com/${s3FileName}`,
        status: "Pushed to Queue",
        claimId, // Store the claimId for future use
      },
    ]);

    setMessage("File uploaded and pushed to SQS successfully!");
    setIsUploaded(true); // Set flag to true after upload
  } catch (error) {
    setMessage("Upload failed: " + error.message);
    console.error("Error during upload:", error);
  } finally {
    setUploading(false);
  }
};




nput for pushing to SQS:
{
   "claimid" : "CL1234567",
   "s3filename" : "aimlusecasesv1/singlifepoc/productsheet/PS123456/R95.01-CancerPremWaiver.pdf",
   "tasktype" : "SEND_TO_QUEUE"
}
 
Output after pushing from SQS:
{
   "message": "Message sent to SQS successfully",
   "messageId": "5a24b46a-488f-4c0e-80b8-4cf33242bb45"
}
