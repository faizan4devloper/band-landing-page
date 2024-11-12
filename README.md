import React from 'react';
import Sidebar from './Sidebar';
import MainContent from './MainContent';

import styles from './JobPage.module.css';

const JobPage = () => {
  return (
    <div className={styles.jobPage}>
      <Sidebar />
      <MainContent />
    </div>
  );
};

export default JobPage;




import React, { useState } from 'react';
import { useDropzone } from 'react-dropzone';
import styles from './Sidebar.module.css';

const Sidebar = () => {
  const [fileName, setFileName] = useState(null);

  const onDrop = (acceptedFiles) => {
    setFileName(acceptedFiles[0].name); // Store the file name of the first file
  };

  const { getRootProps, getInputProps } = useDropzone({ onDrop });

  return (
    <div className={styles.sidebar}>
      <h2 className={styles.sidebarHeader}>Job Application</h2>
      <div {...getRootProps()} className={styles.dropzone}>
        <input {...getInputProps()} />
        <p>Drag & Drop your documents here, or click to select files</p>
      </div>
      {fileName && <p className={styles.fileName}>{fileName}</p>}
    </div>
  );
};

export default Sidebar;




import React from 'react';
import styles from './MainContent.module.css';

const MainContent = () => {
  return (
    <div className={styles.mainContent}>
      <section className={styles.section}>
        <h3>Section 1</h3>
        <p>Details about Section 1</p>
      </section>
      <section className={styles.section}>
        <h3>Section 2</h3>
        <p>Details about Section 2</p>
      </section>
      <section className={styles.section}>
        <h3>Section 3</h3>
        <p>Details about Section 3</p>
      </section>
      <section className={styles.section}>
        <h3>Section 4</h3>
        <p>Details about Section 4</p>
      </section>
      <section className={styles.section}>
        <h3>Section 5</h3>
        <p>Details about Section 5</p>
      </section>
    </div>
  );
};

export default MainContent;



.jobPage {
  display: flex;
  height: 100vh;
  width: 100%;
}



.sidebar {
  width: 300px;
  background-color: #f4f4f4;
  padding: 20px;
  display: flex;
  flex-direction: column;
}

.sidebarHeader {
  font-size: 1.5rem;
  font-weight: bold;
  margin-bottom: 20px;
}

.dropzone {
  border: 2px dashed #cccccc;
  padding: 20px;
  text-align: center;
  border-radius: 8px;
  background-color: #f9f9f9;
  margin-bottom: 20px;
  transition: background-color 0.3s ease;
}

.dropzone:hover {
  background-color: #e9e9e9;
}

.fileName {
  margin-top: 10px;
  font-weight: bold;
  color: #333;
}




.mainContent {
  flex: 1;
  padding: 20px;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
  background-color: #fff;
}

.section {
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease;
}

.section:hover {
  transform: translateY(-5px);
}

h3 {
  margin-bottom: 10px;
  font-size: 1.2rem;
  color: #333;
}

p {
  color: #666;
}
