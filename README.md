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

