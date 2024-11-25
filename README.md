const handleFileChange = (event) => {
  const file = event.target.files[0];
  if (file) {
    // Extract the file extension
    const fileExtension = file.name.split('.').pop();
    // Sanitize the file name and append the extension
    const sanitizedFileName = file.name
      .replace(/\s+/g, '_')
      .replace(/\.[^/.]+$/, '') + `.${fileExtension}`;

    setFileName(sanitizedFileName);

    const renamedFile = new File([file], sanitizedFileName, { type: file.type });
    onFileChange({ target: { files: [renamedFile] } });
  }
  onFileChange(event); // Pass the file to the parent component
};












import React, { useRef, useState } from "react";
import styles from "./Sidebar.module.css";
import { useNavigate } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowLeft } from '@fortawesome/free-solid-svg-icons';




const Sidebar = ({ onFileChange, onUpload, uploading }) => {
  const fileInputRef = useRef(null);
  const [fileName, setFileName] = useState("");
    const navigate = useNavigate();


  // Trigger the file input when the container is clicked
  const handleContainerClick = () => {
    if (fileInputRef.current) {
      fileInputRef.current.click();
    }
  };
  
  const handleBackClick = () => {
    navigate("/datatable");
  };


  // Handle file selection and store the file name
  const handleFileChange = (event) => {
    const file = event.target.files[0];
    if (file) {
      
      const sanitizedFileName = file.name.replace(/\s+/g, '_');
      
      
      setFileName(sanitizedFileName);
      
      const renamedFile = new File([file], sanitizedFileName, { type:file.type });
      onFileChange({ target: { files: [renamedFile]}});
    }
    onFileChange(event); // Pass the file to the parent component
  };

  return (
    <div className={styles.sidebar}>
      <button className={styles.backButton} onClick={handleBackClick}>
        <FontAwesomeIcon icon={faArrowLeft} />
      </button>
      <h2 className={styles.heading}>Upload Product Sheet</h2>

      {/* File Input Container */}
      <div
        className={styles.fileInputContainer}
        onClick={handleContainerClick}
      >
        <p className={styles.dropzoneText}>Click to choose a file or drag & drop</p>
        <input
          type="file"
          ref={fileInputRef}
          onChange={handleFileChange}
          className={styles.fileInput}
          style={{ display: "none" }}
        />
      </div>

      {/* File Name Display */}
      {fileName && (
        <div className={styles.fileNameContainer}>
          <p className={styles.fileName}>{fileName}</p>
        </div>
      )}

      {/* Upload Button */}
      <button
        className={styles.uploadButton}
        onClick={onUpload}
        disabled={uploading}
      >
        {uploading ? (
          <div className={styles.loader}></div>
        ) : (
          "Upload"
        )}
      </button>
    </div>
  );
};

export default Sidebar;








// import React, { useState } from 'react';
// import axios from 'axios';
// import Sidebar from './Sidebar';
// import MainContent from './MainContent';
// import styles from './ProductSheetsPage.module.css';

// const ProductSheetsPage = () => {
//   const [file, setFile] = useState(null);
//   const [message, setMessage] = useState("");
//   const [uploading, setUploading] = useState(false);
//   const [rows, setRows] = useState([]);
//   const [isUploaded, setIsUploaded] = useState(false); // Track if a file has been uploaded

//   const handleFileChange = (e) => {
//     const selectedFile = e.target.files[0];
//     if (selectedFile) {
//       setFile(selectedFile);
//       setMessage(""); // Clear previous messages
//     }
//   };

//   const handleUpload = async () => {
//     if (!file) {
//       setMessage("Please select a file to upload.");
//       return;
//     }
//     setUploading(true);
//     try {
//       // Request the presigned URL from the backend
//       const response = await axios.post("https://41aw3s5s3k.execute-api.us-east-1.amazonaws.com/dev/", {
//         payload: { filename: file.name },
//       });
      
//       console.log(response.data);

//       const { presignedUrl, key, recNum } = response.data;

//       // Use the presigned URL to upload the file
//       await axios.put(presignedUrl, file, {
//         headers: { "Content-Type": file.type },
//       });
      
      
//       // Step 3: Prepare the payload for pushking to SQS
//     const claimId = recNum; // Generate a unique Claim ID
//     const taskType = "SEND_TO_QUEUE";
//     const s3FileName = key; // Use the returned S3 key as the filename

//     const sqsPayload = {
//       claimid: claimId,
//       s3filename: s3FileName,
//       tasktype: taskType,
//     };

//     // Step 4: Send the payload to the new API
//     const sqsResponse = await axios.post("https://e21wxu9skj.execute-api.us-east-1.amazonaws.com/dev/querequest", sqsPayload, {
//       headers: { "Content-Type": "application/json" },
//     });

//     console.log("SQS API Response:", sqsResponse.data);


//       // Add a new row with static RecNum and placeholders
//       // const newRecNum = rows.length + 1;
//       setRows((prevRows) => [
//         ...prevRows,
//         {
//           recNum: recNum,
//           policyid: "",
//           type: "",
//           summary: "",
//           previewLink: presignedUrl,
//           status: "Pending",
//         },
//       ]);
  
//       setMessage("File uploaded successfully!");
//       setIsUploaded(true); // Set flag to true after upload
//     } catch (error) {
//       setMessage("Upload failed: " + error.message);
//     } finally {
//       setUploading(false);
//     }
//   };

//   const handleReload = async (recNum) => {
//     try {
//       const payload = {
//         tasktype: "FETCH_SINGLE_CLAIM",
//         claimid: recNum,
//       };

//       const response = await axios.post(`https://e21wxu9skj.execute-api.us-east-1.amazonaws.com/dev/querequest`, payload, {
//         headers: { "Content-Type": "application/json" },
//       });

//       console.log("Reload Data:", recNum, response.data);
//       const data = response.data;

//       setRows((prevRows) =>
//         prevRows.map((row) =>
//           row.recNum === recNum
//             ? {
//                 ...row,
//                 policyid: data.policyid,
//                 type: data.type,
//                 summary: data.summary,
//                 status: "Completed️✅",
//               }
//             : row
//         )
//       );
//     } catch (error) {
//       setMessage("Failed to fetch data for RecNum: " + recNum);
//       console.error("Reload Error:", error);
//     }
//   };

//   return (
//     <div className={styles.container}>
//       <Sidebar
//         onFileChange={handleFileChange}
//         onUpload={handleUpload}
//         uploading={uploading}
//       />
//       {isUploaded ? ( // Conditionally render MainContent
//         <MainContent
//           message={message}
//           rows={rows}
//           handleReload={handleReload}
//         />
//       ) : (
//         <p className={styles.infoMessage}>
//           Please upload a document to view the data table.
//         </p>
//       )}
//     </div>
//   );
// };

// export default ProductSheetsPage;






import React, { useState } from 'react';
import axios from 'axios';
import Sidebar from './Sidebar';
import MainContent from './MainContent';
import DataTable from './DataTable'; // Import the new DataTable component
import styles from './ProductSheetsPage.module.css';

const ProductSheetsPage = () => {
  const [file, setFile] = useState(null);
  const [message, setMessage] = useState("");
  const [uploading, setUploading] = useState(false);
  const [rows, setRows] = useState([]);
  const [isUploaded, setIsUploaded] = useState(false);
  const [showMainContent, setShowMainContent] = useState(false);

  const handleFileChange = (e) => {
    const selectedFile = e.target.files[0];
    if (selectedFile) {
      setFile(selectedFile);
      setMessage("");
    }
  };

  const handleUpload = async () => {
    if (!file) {
      setMessage("Please select a file to upload.");
      return;
    }
    setUploading(true);
    try {
      const sanitizedFileName = file.name.replace(/\s+/g, '_');
      
      const response = await axios.post("https://41aw3s5s3k.execute-api.us-east-1.amazonaws.com/dev/", {
        payload: { filename: sanitizedFileName },
      });
      
      console.log("API Response 1:", response.data);

      const { presignedUrl, key, recNum } = response.data;

      await axios.put(presignedUrl, file, {
        headers: { "Content-Type": file.type },
      });

      const sqsPayload = {
        claimid: recNum,
        s3filename: key,
        actionn: "transform",
        tasktype: "SEND_TO_PS_QUEUE",
      };

         // Step 4: Send the payload to the new API
    const sqsResponse = await axios.post("https://e21wxu9skj.execute-api.us-east-1.amazonaws.com/dev/querequest", sqsPayload, {
      headers: { "Content-Type": "application/json" },
    });

    console.log("SQS API Response:", sqsResponse.data);

      setRows((prevRows) => [
        ...prevRows,
        {
          recNum,
          policyid: "",
          type: "",
          summary: "",
          previewLink: presignedUrl,
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

  const handleReload = async (recNum) => {
    try {
      const payload = {
        tasktype: "FETCH_SINGLE_CLAIM",
        claimid: recNum,
      };

      const response = await axios.post(`duumy`, payload, {
        headers: { "Content-Type": "application/json" },
      });

      const data = response.data;
      console.log("Reload Data:", response.data);

      setRows((prevRows) =>
        prevRows.map((row) =>
          row.recNum === recNum
            ? {
                ...row,
                policyid: data.policyid,
                type: data.type,
                summary: data.summary,
                status: "Completed️✅",
              }
            : row
        )
      );
    } catch (error) {
      setMessage("Failed to fetch data for RecNum: " + recNum);
    }
  };

  return (
    <div className={styles.container}>
      {!showMainContent ? (
        <>
          {/* New Claim Processing Button */}
          <div className={styles.header}>
            <button
              className={styles.newClaimButton}
              onClick={() => setShowMainContent(true)}
            >
              New Claim Processing
            </button>
          </div>
          <DataTable rows={rows} handleReload={handleReload} /> {/* Use DataTable */}
        </>
      ) : (
        <>
          <Sidebar
            onFileChange={handleFileChange}
            onUpload={handleUpload}
            uploading={uploading}
          />
          {isUploaded ? (
            <MainContent
              message={message}
              rows={rows}
              handleReload={handleReload}
            />
          ) : (
            <p className={styles.infoMessage}>
              Please upload a document to view the data.
            </p>
          )}
        </>
      )}
    </div>
  );
};

export default ProductSheetsPage;
