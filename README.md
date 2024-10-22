// Sidebar.js
import React, { useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faBook, faBriefcase, faHeartbeat, faLaptopCode, faGraduationCap } from '@fortawesome/free-solid-svg-icons';
import styles from './Sidebar.module.css';

const topics = [
  { id: 1, name: 'Courses', icon: faGraduationCap },
  { id: 2, name: 'Jobs', icon: faBriefcase },
  { id: 3, name: 'Health', icon: faHeartbeat },
  { id: 4, name: 'Technology', icon: faLaptopCode },
  { id: 5, name: 'Education', icon: faBook },
];

const Sidebar = ({ setActiveTopic }) => {
  const [active, setActive] = useState(1); // Default active topic

  const handleClick = (id) => {
    setActive(id);
    setActiveTopic(id); // Pass the active topic to the parent component
  };

  return (
    <div className={styles.sidebar}>
      <ul>
        {topics.map((topic) => (
          <li
            key={topic.id}
            className={`${styles.topic} ${active === topic.id ? styles.active : ''}`}
            onClick={() => handleClick(topic.id)}
          >
            <FontAwesomeIcon icon={topic.icon} className={styles.icon} />
            <span>{topic.name}</span>
          </li>
        ))}
      </ul>
    </div>
  );
};

export default Sidebar;



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


// App.js
import React, { useState } from 'react';
import Sidebar from './components/Sidebar/Sidebar';
import MainContent from './components/MainContent/MainContent';
import styles from './App.module.css';

function App() {
  const [activeTopic, setActiveTopic] = useState(1); // Default active topic

  return (
    <div className={styles.container}>
      <Sidebar setActiveTopic={setActiveTopic} />
      <MainContent activeTopic={activeTopic} />
    </div>
  );
}

export default App;



.sidebar {
  width: 250px;
  background-color: #2c3e50;
  padding: 20px;
  height: 100vh;
  color: white;
}

.sidebar ul {
  list-style-type: none;
  padding: 0;
}

.sidebar li {
  display: flex;
  align-items: center;
  padding: 10px;
  cursor: pointer;
  border-radius: 5px;
  margin-bottom: 15px;
  transition: background-color 0.3s;
}

.sidebar li:hover {
  background-color: #34495e;
}

.sidebar li.active {
  background-color: #2980b9;
}

.icon {
  margin-right: 10px;
  font-size: 1.5em;
}
