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

//   const handleFileChange = (e) => {
//     const selectedFile = e.target.files[0];
//     if (selectedFile) {
//       setFile(selectedFile);
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
      
//       console.log(response.data)

//       const { presignedUrl, key } = response.data;

//       // Use the presigned URL to upload the file
//       await axios.put(presignedUrl, file, {
//         headers: { "Content-Type": file.type },
//       });

//       // Add a new row with static RecNum and placeholders
//       const newRecNum = rows.length + 1;
//       setRows((prevRows) => [
//         ...prevRows,
//         {
//           recNum: newRecNum,
//           policyid: "",
//           type: "",
//           summary: "",
//           previewLink: `https://your-s3-bucket-url.com/${key}`,
//           status: "Pending",
//         },
//       ]);
  
//       setMessage("File uploaded successfully!");
//     } catch (error) {
//       setMessage("Upload failed: " + error.message);
//     } finally {
//       setUploading(false);
//     }
//   };

 
// const handleReload = async (recNum) => {
//   try {
//     // Define the payload with the required properties
//     const payload = {
//       recNum, // Add the RecNum value dynamically
//       fileType: file ? file.type : "application/json", // Example: dynamic or default type
//       filename: file ? file.name : "example.txt", // Example: dynamic or default filename
//     };

//     // Make the API call with the payload
//     const response = await axios.post(`https://41aw3s5s3k.execute-api.us-east-1.amazonaws.com/dev/fileprocessing`, payload, {
//       headers: { "Content-Type": "application/json" },
//     });
    


//     console.log("Reload Data:", recNum, response.data);
//     const data = response.data;

//     // Update the row with the fetched data
//     setRows((prevRows) =>
//       prevRows.map((row) =>
//         row.recNum === recNum
//           ? {
//               ...row,
//               policyid: data.policyid,
//               type: data.type,
//               summary: data.summary,
//               status: "Completed✔️",
//             }
//           : row
//       )
//     );
//   } catch (error) {
//     setMessage("Failed to fetch data for RecNum: " + recNum);
//     console.error("Reload Error:", error);
//   }
// };

//   return (
//     <div className={styles.container}>
//       <Sidebar onFileChange={handleFileChange} onUpload={handleUpload} uploading={uploading} />
//       <MainContent message={message} rows={rows} handleReload={handleReload} />
//     </div>
//   );
// };

// export default ProductSheetsPage;



import React, { useState } from 'react';
import axios from 'axios';
import Sidebar from './Sidebar';
import MainContent from './MainContent';
import styles from './ProductSheetsPage.module.css';

const ProductSheetsPage = () => {
  const [file, setFile] = useState(null);
  const [message, setMessage] = useState("");
  const [uploading, setUploading] = useState(false);
  const [rows, setRows] = useState([]);
  const [isUploaded, setIsUploaded] = useState(false); // Track if a file has been uploaded

  const handleFileChange = (e) => {
    const selectedFile = e.target.files[0];
    if (selectedFile) {
      setFile(selectedFile);
      setMessage(""); // Clear previous messages
    }
  };

  const handleUpload = async () => {
    if (!file) {
      setMessage("Please select a file to upload.");
      return;
    }
    setUploading(true);
    try {
      // Request the presigned URL from the backend
      const response = await axios.post("https://41aw3s5s3k.execute-api.us-east-1.amazonaws.com/dev/", {
        payload: { filename: file.name },
      });
      
      console.log(response.data);

      const { presignedUrl, key } = response.data;

      // Use the presigned URL to upload the file
      await axios.put(presignedUrl, file, {
        headers: { "Content-Type": file.type },
      });

      // Add a new row with static RecNum and placeholders
      const newRecNum = rows.length + 1;
      setRows((prevRows) => [
        ...prevRows,
        {
          recNum: newRecNum,
          policyid: "",
          type: "",
          summary: "",
          previewLink: `https://your-s3-bucket-url.com/${key}`,
          status: "Pending",
        },
      ]);
  
      setMessage("File uploaded successfully!");
      setIsUploaded(true); // Set flag to true after upload
    } catch (error) {
      setMessage("Upload failed: " + error.message);
    } finally {
      setUploading(false);
    }
  };

  const handleReload = async (recNum) => {
    try {
      const payload = {
        recNum,
        fileType: file ? file.type : "application/json",
        filename: file ? file.name : "example.txt",
      };

      const response = await axios.post(`https://41aw3s5s3k.execute-api.us-east-1.amazonaws.com/dev/fileprocessing`, payload, {
        headers: { "Content-Type": "application/json" },
      });

      console.log("Reload Data:", recNum, response.data);
      const data = response.data;

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
      console.error("Reload Error:", error);
    }
  };

  return (
    <div className={styles.container}>
      <Sidebar
        onFileChange={handleFileChange}
        onUpload={handleUpload}
        uploading={uploading}
      />
      {isUploaded ? ( // Conditionally render MainContent
        <MainContent
          message={message}
          rows={rows}
          handleReload={handleReload}
        />
      ) : (
        <p className={styles.infoMessage}>
          Please upload a document to view the data table.
        </p>
      )}
    </div>
  );
};

export default ProductSheetsPage;






import React, { useState } from 'react';
import Modal from 'react-modal';
import styles from './MainContent.module.css';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faRotateRight } from '@fortawesome/free-solid-svg-icons';

const MainContent = ({ message, rows, handleReload }) => {
  const [filter, setFilter] = useState("All");
  const [isModalOpen, setIsModalOpen] = useState(false);
  const [modalContent, setModalContent] = useState(null);

  // Filter rows based on the selected filter
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

      {/* Filter Dropdown */}
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

      {/* Table */}
      <table className={styles.table}>
        <thead>
          <tr>
            <th>RecNum</th>
            <th>Policy ID</th>
            <th>Product Sheet Type</th>
            <th>Summary</th>
            <th>Preview Link</th>
            <th>Status</th>
          </tr>
        </thead>
        <tbody>
          {filteredRows.length > 0 ? (
            filteredRows.map((row, index) => (
              <tr key={index}>
                <td>{row.recNum}</td>
                <td>{row.policyid || <span className={styles.loader}>Pending</span>}</td>
                <td>{row.type || <span className={styles.loader}>Pending</span>}</td>
                <td>{row.summary || <span className={styles.loader}>Pending</span>}</td>
                <td>
                  {row.previewLink ? (
                    <button
                      className={styles.previewButton}
                      onClick={() => openModal(row.previewLink)}
                    >
                      Preview
                    </button>
                  ) : (
                    "Pending"
                  )}
                </td>
                <td>
                  {row.status === "Pending" ? (
                    <span>
                      Pending
                      <button
                        className={styles.reloadButton}
                        onClick={() => handleReload(row.recNum)}
                      >
                        <FontAwesomeIcon icon={faRotateRight} />
                      </button>
                    </span>
                  ) : (
                    row.status
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

      {/* Modal */}
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
