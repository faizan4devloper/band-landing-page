import React, { useState } from "react";
import Sidebar from "./Sidebar"; // Import the Sidebar component

const ParentComponent = () => {
  const [file, setFile] = useState(null); // State to store the selected file
  const [uploading, setUploading] = useState(false); // State to track the upload status

  // Handle file selection (from Sidebar)
  const handleFileChange = (acceptedFiles) => {
    // Store the selected file in state
    setFile(acceptedFiles[0]);
    console.log("File selected:", acceptedFiles[0]); // Log for debugging
  };

  // Handle file upload
  const handleUpload = () => {
    if (!file) {
      alert("Please select a file first!");
      return;
    }

    // Simulate a file upload (you should replace this with actual API call)
    setUploading(true); // Set uploading state to true to disable the button
    console.log("Uploading file:", file.name);

    // Example: Simulating an upload with setTimeout
    setTimeout(() => {
      setUploading(false); // Set uploading state to false once the upload is complete
      alert("File uploaded successfully!");
      console.log("Upload complete"); // Log when upload is complete
    }, 2000); // Simulate a 2-second upload delay
  };

  return (
    <Sidebar
      onFileChange={handleFileChange}
      onUpload={handleUpload}
      uploading={uploading}
    />
  );
};

export default ParentComponent;
