import React, { useRef, useState } from "react";
import styles from "./Sidebar.module.css";
import { useNavigate } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowLeft } from '@fortawesome/free-solid-svg-icons';

const Sidebar = ({ onFileChange, onUpload, uploading, onPolicyChange, setFileName }) => {
  const fileInputRef = useRef(null);
  const navigate = useNavigate();

  // Handle file selection and update the parent component's fileName state
  const handleFileChange = (event) => {
    const file = event.target.files[0];
    if (file) {
      const sanitizedFileName = file.name.replace(/\s+/g, '_');
      setFileName(sanitizedFileName); // Lift the file name up to MainContent
      onFileChange(event); // Pass the file to the parent component
    }
  };

  // Trigger the file input when the container is clicked
  const handleContainerClick = () => {
    if (fileInputRef.current) {
      fileInputRef.current.click();
    }
  };

  // Navigate back to the claims page
  const handleBackClick = () => {
    navigate("/claims");
  };

  return (
    <div className={styles.sidebar}>
      <button className={styles.backButton} onClick={handleBackClick}>
        <FontAwesomeIcon icon={faArrowLeft} />
      </button>
      <h2 className={styles.heading}>Manage Claim</h2>

      {/* File Input Container */}
      <div className={styles.fileInputContainer} onClick={handleContainerClick}>
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
      {setFileName && (
        <div className={styles.fileNameContainer}>
          <p className={styles.fileName}>{setFileName}</p>
        </div>
      )}

      {/* Upload Button */}
      <button
        className={styles.uploadButton}
        onClick={onUpload}
        disabled={uploading}
      >
        {uploading ? <div className={styles.loader}></div> : "Upload"}
      </button>
    </div>
  );
};

export default Sidebar;








const Sidebar = ({ onFileChange, onUpload, uploading, onPolicyChange, setFileName }) => {
  const fileInputRef = useRef(null);

  const handleFileChange = (event) => {
    const file = event.target.files[0];
    if (file) {
      const sanitizedFileName = file.name.replace(/\s+/g, '_');
      setFileName(sanitizedFileName); // Lift state up
      onFileChange(event);
    }
  };

  return (
    <div className={styles.sidebar}>
      <input
        type="file"
        ref={fileInputRef}
        onChange={handleFileChange}
        style={{ display: "none" }}
      />
      {fileName && <p>{fileName}</p>}
    </div>
  );
};

export default Sidebar;









const MainContent = ({ message, rows, clid, setRows, staticPreviewUrl, selectedPolicy }) => {
  const [fileName, setFileName] = useState(""); // New state for file name
  const [data, setData] = useState(null);

  return (
    <div className={styles.mainContentWrapper}>
      <Sidebar setFileName={setFileName} />
      <ClaimClassification data={data} fileName={fileName} />
    </div>
  );
};

export default MainContent;











const ClaimClassification = ({ data, fileName }) => {
  const [documentName, setDocumentName] = useState('');

  useEffect(() => {
    if (fileName) {
      setDocumentName(fileName); // Prefer uploaded file name if available
    } else if (data?.total_extracted_data) {
      try {
        const parsedData = JSON.parse(data.total_extracted_data);
        setDocumentName(parsedData.CLAIM_FORM_DETAILS?.DOCUMENT_NAME || 'Unnamed Document');
      } catch (error) {
        setDocumentName('Unknown Document');
      }
    }
  }, [data, fileName]); // React when `fileName` changes

  return (
    <table>
      <thead>
        <tr>
          <th>Document Name</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>{documentName}</td>
        </tr>
      </tbody>
    </table>
  );
};

export default ClaimClassification;
