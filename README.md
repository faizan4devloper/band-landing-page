import React, { useState} from 'react';
import Sidebar from './Sidebar';
import MainContent from './MainContent';

import styles from './EducationPage.module.css';

const EducationPage = () => {
  const [activeTopic, setActiveTopic] = useState(1);
  
  return (
    <div className={styles.educationPage}>
      <Sidebar setActiveTopic={setActiveTopic} />
      <MainContent activeTopic={activeTopic} />
    </div>
  );
};

export default EducationPage;
