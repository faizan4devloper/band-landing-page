import React, { useState } from "react";
import axios from "axios";
import Sidebar from "./Sidebar";
import MainContent from "./MainContent";
import Verify from "./Verify";
import styles from "./NewClaimPage.module.css";
// import { useNavigate } from "react-router-dom";

const NewClaimPage = () => {
  const [file, setFile] = useState(null);
  const [message, setMessage] = useState("");
  
  
  const [uploading, setUploading] = useState(false);
  const [rows, setRows] = useState([]);
  const [isUploaded, setIsUploaded] = useState(false);
  const [previewUrl, setPreviewUrl] = useState(""); // State for static preview URL
    const [selectedPolicy, setSelectedPolicy] = useState(""); // State for policy selection
    // const navigate = useNavigate()


 
  const handleFileChange = (e) => {
    const selectedFile = e.target.files[0];
    if (selectedFile) {
      setFile(selectedFile);
            setPreviewUrl(URL.createObjectURL(selectedFile)); // Generate preview URL

      setMessage("");
    }
  };
  
  
   const handlePolicyChange = (policy) => {
    setSelectedPolicy(policy);
    console.log("Selected Policy:", policy);
  };
  
  // const navigateToVerify = ()=>{
  //   navigate("/verify", {state: {selectedPolicy, rows}})
  // }


  const handleUpload = async () => {
    if (!file) {
      setMessage("Please select a file to upload.");
      return;
    }
    
    
    setUploading(true);
    try {
      const sanitizedFileName = file.name.replace(/\s+/g, "_");
      const response = await axios.post(
        "https://41aw3s5s3k.execute-api.us-east-1.amazonaws.com/dev/",
        {
          payload: { filename: sanitizedFileName, filetype: "CL" },
        }
      );
      
      console.log("Presign url", response.data)

      const { presignedUrl, key, recNum } = response.data;
      
      console.log("**Response Api",response.data)

      await axios.put(presignedUrl, file, {
        headers: { "Content-Type": file.type },
      });

      const sqsPayload = {
        claimid: recNum,
        s3filename: key,
        tasktype: "SEND_TO_QUEUE",
      };

    const sqsResponse=  await axios.post(
        "https://e21wxu9skj.execute-api.us-east-1.amazonaws.com/dev/querequest",
        sqsPayload,
        { headers: { "Content-Type": "application/json" } }
      );
      
      console.log("SQS API Response:", sqsResponse.data);

      setRows((prevRows) => [
        ...prevRows,
        {
          recNum,
          policyid: "",
          type: "",
          summary: "",
          previewLink: presignedUrl,
         policy: selectedPolicy, // Include the selected policy

          status: "Pending",
        },
      ]);

      setIsUploaded(true);
    } catch (error) {
      setMessage("Upload failed: " + error.message);
    } finally {
      setUploading(false);
    }
  };

  return (
    <div className={styles.container}>
      <div className={styles.sidebar}>
        <Sidebar
          onFileChange={handleFileChange}
          onUpload={handleUpload}
          uploading={uploading}
         onPolicyChange={handlePolicyChange}

        />
      </div>
      <div className={styles.mainContent}>
        {isUploaded ? (
          <MainContent 
            message={message} 
            selectedPolicy={selectedPolicy}
            rows={rows} 
            clid={rows[0].recNum}
            setRows={setRows} 
            staticPreviewUrl={previewUrl} // Pass the static preview URL
          />
        ) : (
          <p className={styles.infoMessage}>
            Please upload a document to view the data.
          </p>
        )}
      </div>
    </div>
  );
};

export default NewClaimPage;
