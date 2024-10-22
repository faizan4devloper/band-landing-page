// MainContent.js
import React from 'react';
import styles from './MainContent.module.css';

const contentData = {
  1: 'Here you can explore various courses to enhance your knowledge.',
  2: 'Explore job opportunities to advance your career.',
  3: 'Manage your health records and access health services.',
  4: 'Stay updated with the latest in technology and innovations.',
  5: 'Find education resources and tools for academic success.',
};

const MainContent = ({ activeTopic }) => {
  return (
    <div className={styles.mainContent}>
      <h2>{contentData[activeTopic]}</h2>
    </div>
  );
};

export default MainContent;




.mainContent {
  flex-grow: 1;
  padding: 30px;
  background-color: #ecf0f1;
}

.mainContent h2 {
  font-size: 2em;
  margin-bottom: 20px;
}
