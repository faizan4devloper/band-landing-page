
const handleUpload = async () => {
  if (!file) {
    setMessage("Please select a file to upload.");
    return;
  }

  setUploading(true);

  try {
    const sanitizedFileName = file.name.replace(/\s+/g, '_');
    console.log("Uploading file:", sanitizedFileName);

    // Step 1: Request presigned URL
    const response = await axios.post("dummy", {
      payload: { filename: sanitizedFileName },
    });

    console.log("Presigned URL Response:", response.data);

    let { presignedUrl, key, recNum } = response.data;

    // Prepend "ps" to recNum
    recNum = `ps${recNum}`;

    // Step 2: Upload the file to S3 using the presigned URL
    await axios.put(presignedUrl, file, {
      headers: { "Content-Type": file.type || "application/pdf" },
    });

    console.log("File uploaded successfully to S3.");

    // Step 3: Notify backend for further processing
    const sqsPayload = {
      claimid: recNum, // Ensure claimid uses "ps" prepended recNum
      psfilename: key, // Updated to use `ps` instead of `cl` and `ci`
      actionn: "transform",
      tasktype: "SEND_TO_PS_QUEUE",
    };

    const sqsResponse = await axios.post("dummy", sqsPayload, {
      headers: { "Content-Type": "application/json" },
    });

    console.log("SQS API Response:", sqsResponse.data);

    // Update rows with the new record
    setRows((prevRows) => [
      ...prevRows,
      {
        recNum, // Display the modified "ps"-prefixed recNum
        policyid: "",
        type: "",
        summary: "",
        previewLink: presignedUrl,
        status: "Pending",
      },
    ]);

    setIsUploaded(true);
  } catch (error) {
    console.error("Upload failed:", error);
    setMessage("Upload failed: " + (error.response?.data?.message || error.message));
  } finally {
    setUploading(false);
  }
};
