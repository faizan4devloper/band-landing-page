// MainContent.js
import React from 'react';
import styles from './MainContent.module.css';

const contentData = {
  1: {
    title: "Education Resources",
    description: "Detailed educational content...",
  },
  2: {
    title: "Job Opportunities",
    description: "Explore job listings...",
  },
  3: {
    title: "Health Services",
    description: "Manage your health records...",
  },
  4: {
    title: "Technology Innovations",
    description: "Learn about tech advancements...",
  },
  5: {
    title: "Learning Platforms",
    description: "Access learning resources...",
  },
};

const MainContent = ({ activeTopic }) => {
  if (!activeTopic) {
    return <div className={styles.mainContent}>Please select a topic from the sidebar.</div>;
  }

  const content = contentData[activeTopic];

  return (
    <div className={styles.mainContent}>
      <h1>{content.title}</h1>
      <p>{content.description}</p>
    </div>
  );
};

export default MainContent;



import React, { useState } from 'react';
import Sidebar from '../Sidebar/Sidebar'; // Sidebar component
import MainContent from '../MainContent/MainContent'; // MainContent component
import styles from './EducationPage.module.css';

const EducationPage = () => {
  const [activeTopic, setActiveTopic] = useState(null); // Track the active topic

  return (
    <div className={styles.educationPage}>
      <Sidebar setActiveTopic={setActiveTopic} /> {/* Pass setActiveTopic to Sidebar */}
      <MainContent activeTopic={activeTopic} /> {/* Pass activeTopic to MainContent */}
    </div>
  );
};

export default EducationPage;


/* EducationPage.module.css */
.educationPage {
  display: flex;
  height: 100vh;
}

/* Sidebar.module.css */
.sidebar {
  width: 250px;
  background-color: #f7f7f7;
  padding: 20px;
  display: flex;
  flex-direction: column;
}

.sidebar ul {
  list-style-type: none;
  padding: 0;
}

.sidebar li {
  padding: 10px 15px;
  margin-bottom: 10px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.sidebar li.active {
  background-color: #e2d9fb;
}

.sidebar li:hover {
  background-color: #ddd;
}

.icon {
  margin-right: 10px;
}

/* MainContent.module.css */
.mainContent {
  flex: 1;
  padding: 20px;
  background-color: #fff;
}

