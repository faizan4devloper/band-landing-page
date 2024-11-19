const handleUpload = async () => {
  if (!file) {
    setMessage("Please select a file to upload.");
    return;
  }

  try {
    const payload = {
      payload: {
        filename: file.name,
      },
    };

    // Request pre-signed URL from API Gateway
    const { data } = await axios.post("https://<your-api-gateway-url>", payload, {
      headers: { "Content-Type": "application/json" },
    });

    console.log("API Response:", data);

    // Use the correct key for the pre-signed URL
    const url = data.presignedUrl;
    if (!url || !/^https?:\/\//.test(url)) {
      throw new Error("Invalid pre-signed URL received");
    }

    // Upload the file to S3
    await axios.put(url, file, {
      headers: { "Content-Type": file.type },
    });

    setProductSheets((prevSheets) => [
      ...prevSheets,
      {
        fileName: file.name,
        fileType: file.type,
        status: "Uploaded Successfully",
      },
    ]);

    setMessage("File uploaded successfully!");
    setFile(null); // Clear the file input
  } catch (error) {
    console.error("Upload failed:", error.message);
    setMessage("Failed to upload the file.");
    setProductSheets((prevSheets) => [
      ...prevSheets,
      {
        fileName: file.name,
        fileType: file.type,
        status: "Upload Failed",
      },
    ]);
  }
};
