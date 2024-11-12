import React, { useState } from 'react';
import { useDropzone } from 'react-dropzone';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowUpFromBracket } from '@fortawesome/free-solid-svg-icons'; // Import FontAwesome left arrow icon
import styles from './Sidebar.module.css';

const Sidebar = () => {
  const [fileName, setFileName] = useState(null);

  const onDrop = (acceptedFiles) => {
    setFileName(acceptedFiles[0].name); // Store the file name of the first file
  };

  const { getRootProps, getInputProps } = useDropzone({ onDrop });

  return (
    <div className={styles.sidebar}>
      <h2 className={styles.sidebarHeader}>Document Uploaded</h2>
      <div {...getRootProps()} className={styles.dropzone}>
        <input {...getInputProps()} />
        <p>Drag & Drop, or click to select files</p>
        <FontAwesomeIcon icon={faArrowUpFromBracket} className={styles.uploadIcon}/>
      </div>
      {fileName && <p className={styles.fileName}>{fileName}</p>}
    </div>
  );
};

export default Sidebar;



.sidebar {
     width: 250px;
    border-right: 1px solid rgba(0, 0, 0, 0.1);
    color: #333;
    padding: 20px;
    height: 100%;
    overflow-y: hidden;
}

.sidebarHeader {
  font-size: 1rem;
  text-align: center;
  font-weight: bold;
  margin-bottom: 20px;
}

.dropzone {
  border: 2px dashed #cccccc;
  padding: 5px;
  font-size: 12px;
  cursor: pointer;
  text-align: center;
  border-radius: 8px;
  background: rgba(230, 235, 245, 1); /* HCLTech theme hover color */
  margin-bottom: 20px;
  transition: background-color 0.3s ease;
}

.dropzone:hover {
  background:linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%);
  color: #fff;
}

.fileName {
  margin-top: 10px;
  font-size: 14px;
  /*font-weight: bold;*/
  text-align: center;
  color: #333;
}

.uploadIcon{
    font-size: 18px;
}
