style the data and add perfect fonts size and consistent and add perfect loader icon and effect perfect the alignment remove the recnum and add preview i mean add link after i click then open beutiful modal and in that that pdf in iframe

import React, { useState } from 'react';
import axios from 'axios';
import Modal from 'react-modal';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faRotateRight, faSpinner } from '@fortawesome/free-solid-svg-icons';

const MainContent = ({ message, rows, setRows }) => {
  const [filter, setFilter] = useState("All");
  const [isModalOpen, setIsModalOpen] = useState(false);
  const [modalContent, setModalContent] = useState(null);
  const [loadingRows, setLoadingRows] = useState([]);

  const handleReload = async (recNum) => {
    setLoadingRows((prev) => [...prev, recNum]); // Add the row to the loading state
    try {
      const payload = {
        tasktype: "FETCH_SINGLE_CLAIM",
        claimid: recNum,
      };

      const response = await axios.post(`dummy`, payload, {
        headers: { "Content-Type": "application/json" },
      });

      const singleclaimdata = response.data.singleclaimdata[0] || {};
      console.log("Reload Data:", response.data);
      // console.log("single Data:", singleclaimdata.data);
      console.log("policy Data:", response.data.singleclaimdata[0].policy_id);

      setRows((prevRows) =>
        prevRows.map((row) =>
          row.recNum === recNum
            ? {
                ...row,
                policy_id: singleclaimdata.policy_id,
                prod_sheet_type: singleclaimdata.prod_sheet_type,
                summary: singleclaimdata.summary,
                status: singleclaimdata.status,
                previewLink: singleclaimdata.previewLink,
              }
            : row
        )
      );
      console.log("Update rows:", rows)
      
    } catch (error) {
      console.error("Failed to fetch data for RecNum:", recNum, error);
    } finally {
      setLoadingRows((prev) => prev.filter((id) => id !== recNum)); // Remove the row from the loading state
    }
  };

  const filteredRows = rows.filter((row) =>
    filter === "All" ? true : row.status.includes(filter)
  );

  const handleFilterChange = (e) => {
    setFilter(e.target.value);
  };

  const openModal = (previewLink) => {
    setModalContent(previewLink);
    setIsModalOpen(true);
  };

  const closeModal = () => {
    setIsModalOpen(false);
    setModalContent(null);
  };

  return (
    <div className={styles.mainContent}>
      {message && <p className={styles.message}>{message}</p>}

      <div className={styles.filterContainer}>
        <label htmlFor="statusFilter" className={styles.filterLabel}>
          Filter by Status:
        </label>
        <select
          id="statusFilter"
          className={styles.filterDropdown}
          value={filter}
          onChange={handleFilterChange}
        >
          <option value="All">All</option>
          <option value="Pending">Pending</option>
          <option value="Completed">Completed</option>
        </select>
      </div>

      <table className={styles.table}>
        <thead>
          <tr>
            <th>RecNum</th>
            <th>Policy ID</th>
            <th>Product Sheet Type</th>
            <th>Summary</th>
            <th>Status</th>
          </tr>
        </thead>
        <tbody>
          {filteredRows.length > 0 ? (
            filteredRows.map((row, index) => (
              <tr key={index}>
                <td>{row.recNum || <span className={styles.loader}>Pending</span>}</td>
                <td>{row.policy_id || <span className={styles.loader}>Pending</span>}</td>
                <td>{row.prod_sheet_type || <span className={styles.loader}>Pending</span>}</td>
                <td>{row.summary || <span className={styles.loader}>Pending</span>}</td>
                
                <td>
                  {row.policy_id && row.prod_sheet_type && row.summary ? (
                    <span>Completed</span>
                  ) : (
                    <span>
                      <button
                        className={styles.reloadButton}
                        onClick={() => handleReload(row.recNum)}
                        disabled={loadingRows.includes(row.recNum)}
                      >
                        {loadingRows.includes(row.recNum) ? (
                          <FontAwesomeIcon
                            icon={faSpinner}
                            spin
                            className={styles.spinnerIcon}
                          />
                        ) : (
                          <FontAwesomeIcon icon={faRotateRight} />
                        )}
                      </button>
                    </span>
                  )}
                </td>
              </tr>
            ))
          ) : (
            <tr>
              <td colSpan="6">No data available</td>
            </tr>
          )}
        </tbody>
      </table>

      <Modal
        isOpen={isModalOpen}
        onRequestClose={closeModal}
        className={styles.modal}
        overlayClassName={styles.modalOverlay}
        ariaHideApp={false}
      >
        <div className={styles.modalContent}>
          <button className={styles.closeButton} onClick={closeModal}>
            Close
          </button>
          {modalContent ? (
            <iframe
              src={modalContent}
              title="Preview"
              className={styles.modalIframe}
            ></iframe>
          ) : (
            <p>No preview available</p>
          )}
        </div>
      </Modal>
    </div>
  );
};

export default MainContent;



/* Main Content Styles */
.mainContent {
  margin-left: 20px;
  flex-grow: 1;
  padding: 20px;
}

/* Message Styling */
.message {
  padding: 10px;
  background-color: #f9f9f9;
  border-left: 5px solid #007bff;
  margin-bottom: 20px;
  font-size: 16px;
  color: #333;
}


.table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
  font-size: 16px; /* Improved font size for readability */
  color: #333; /* Text color */
  background-color: #ffffff; /* White background for clean design */
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1); /* Subtle shadow for table */
}

.table th,
.table td {
  padding: 15px; /* Increased padding for better spacing */
  border: 1px solid #ddd; /* Subtle border */
  text-align: left; /* Align text to the left for consistency */
}

.table th {
  background: linear-gradient(135deg, #4a90e2, #357ae8); /* Gradient background */
  color: #fff; /* White text for contrast */
  font-weight: 600; /* Bolder text */
  text-transform: uppercase; /* Uppercase headers for distinction */
  font-size: 14px; /* Slightly smaller font for headers */
}

.table tr:nth-child(even) {
  background-color: #f8f9fa; /* Light grey for alternate rows */
}

.table tr:hover {
  background-color: #e8f0fe; /* Subtle blue hover effect */
}

.table td {
     font-size: 13px;
    font-weight: 450;
    line-height: 1.5;
}

.reloadButton {
  background: none;
  border: none;
  color: #357ae8;
  cursor: pointer;
  margin: 0;
  padding: 6px;
  font-size: 14px;
  transition: color 0.3s ease;
}

.reloadButton:hover {
  color: #1d5dbf; /* Darker blue for hover state */
}

.previewButton {
  background-color: #4a90e2;
  color: white;
  border: none;
  padding: 8px 12px;
  cursor: pointer;
  border-radius: 4px;
  transition: background-color 0.3s ease;
  font-size: 14px; /* Consistent font size */
}

.previewButton:hover {
  background-color: #357ae8; /* Darker blue hover effect */
}

/* Empty Table Message */
.table-empty {
  text-align: center;
  color: #999;
  font-size: 16px;
  padding: 20px;
}

/* Loader Style */
.loader {
  font-style: italic;
  color: #aaa;
}

/* Modal Close Button */
.closeButton {
  background-color: #ff4d4f;
  color: white;
  border: none;
  padding: 8px 12px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  float: right;
  font-size: 14px;
}

.closeButton:hover {
  background-color: #d9363e;
}

.previewButton:hover {
  background-color: #0056b3;
}

/* Modal Styling */
.modalOverlay {
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  background: white;
  width: 80%;
  max-width: 600px;
  border-radius: 8px;
  overflow: hidden;
}

.modalContent {
  padding: 20px;
}

.modalIframe {
  width: 100%;
  height: 400px;
  border: none;
}

.closeButton {
  background: red;
  color: white;
  border: none;
  padding: 6px 10px;
  cursor: pointer;
  border-radius: 4px;
  float: right;
}

.closeButton:hover {
  background: darkred;
}

.filterContainer{
  margin-bottom: 20px;
  display: flex;
  align-items: center;
  gap: 10px;
}

.filterLabel{
  font-size: 16px;
  font-weight: 500;
}

.filterDropdown{
     padding: 8px 12px;
    border: 1.5px solid #ccc;
    border-radius: 4px;
    cursor: pointer;
    flex-wrap: wrap;

}

@keyframes spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

.reloadButton .spinning {
  animation: spin 1s linear infinite;
}


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
  
  
//   const handleFileChange = (event) => {
//   const file = event.target.files[0];
//   if (file) {
//     // Extract the file extension
//     const fileExtension = file.name.split('.').pop();
//     // Sanitize the file name and append the extension
//     const sanitizedFileName = file.name
//       .replace(/\s+/g, '_')
//       .replace(/\.[^/.]+$/, '') + `.${fileExtension}`;

//     setFileName(sanitizedFileName);

//     const renamedFile = new File([file], sanitizedFileName, { type: file.type });
//     onFileChange({ target: { files: [renamedFile] } });
//   }
//   onFileChange(event); // Pass the file to the parent component
// };

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



import React, { useState } from 'react';
import axios from 'axios';
import Sidebar from './Sidebar';
import MainContent from './MainContent';
import DataTable from './DataTable'; // Import the new DataTable component
import styles from './ProductSheetsPage.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { 
 faAnglesRight
} from '@fortawesome/free-solid-svg-icons';


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
      const response = await axios.post("dummy", {
        payload: { filename: sanitizedFileName, filetype:"PS" },
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

      const sqsResponse = await axios.post("dummy", sqsPayload, {
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
      {!showMainContent ? (
        <>
           

            <button
              className={styles.newClaimButton}
              onClick={() => setShowMainContent(true)}
            >
              New Product Sheet <FontAwesomeIcon icon={faAnglesRight}/>
            </button>
            <br />
          <div className={styles.header}>
            <h1 className={styles.title}>Product Sheet Dashboard</h1>
          </div>
          <DataTable rows={rows} /> {/* Use DataTable */}
        </>
      ) : (
        <>
          <Sidebar
            onFileChange={handleFileChange}
            onUpload={handleUpload}
            uploading={uploading}
          />
          {isUploaded ? (
            <MainContent message={message} rows={rows} setRows={setRows} />
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

