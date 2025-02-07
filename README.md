const handleUpload = async () => {
  if (!selectedFiles || selectedFiles.length === 0) {
    alert('Please select at least one file.');
    return;
  }

  try {
    setUploadProgress(0);
    setUploadStatus('Uploading...');

    const uploadedFiles = [];
    const failedFiles = [];

    for (let file of selectedFiles) {
      try {
        const payload = {
          category: "AccountStatement", // Fixed category as per your request
          filename: file.name, // Use actual filename
        };

        console.log("Uploading File:", payload); // Debugging log

        // Step 1: Request presigned URL from backend
        const urlResponse = await axios.post(
          'https://your-backend-endpoint.com/file-upload', // Replace with actual API
          payload,
          { headers: { 'Content-Type': 'application/json' } }
        );

        const { presignedUrl, key, recNum, bucket } = urlResponse.data;

        // Step 2: Upload file to S3 using presigned URL
        await axios.put(presignedUrl, file, {
          headers: { 'Content-Type': file.type },
          onUploadProgress: (progressEvent) => {
            const percentCompleted = Math.round(
              (progressEvent.loaded * 100) / progressEvent.total
            );
            setUploadProgress(percentCompleted);
          },
        });

        uploadedFiles.push({ fileName: file.name, recordNumber: recNum, s3Key: key, bucket });
      } catch (error) {
        console.error(`Failed to upload file: ${file.name}`);
        if (error.response) {
          console.error('Response Data:', error.response.data);
          console.error('Status Code:', error.response.status);
        } else {
          console.error('Error Message:', error.message);
        }
        failedFiles.push(file.name);
      }
    }

    // Update upload status
    if (failedFiles.length === 0) {
      setUploadStatus('All files uploaded successfully!');
    } else if (failedFiles.length === selectedFiles.length) {
      setUploadStatus('All uploads failed.');
    } else {
      setUploadStatus(
        `Partial success: ${uploadedFiles.length} files uploaded, ${failedFiles.length} failed.`
      );
    }

    // Reset after upload
    setSelectedFiles(null);
    setUploadProgress(0);

  } catch (error) {
    console.error('Error during upload', error);
    setUploadStatus('Upload failed: An unexpected error occurred.');
  }
};
