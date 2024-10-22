// Sidebar.js
import React, { useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faBook, faBriefcase, faHeartbeat, faLaptopCode, faGraduationCap } from '@fortawesome/free-solid-svg-icons';
import styles from './Sidebar.module.css';

const topics = [
  { id: 1, name: 'Topic 1', icon: faGraduationCap },
  { id: 2, name: 'Topic 2', icon: faBriefcase },
  { id: 3, name: 'Topic 3', icon: faHeartbeat },
  { id: 4, name: 'Topic 4', icon: faLaptopCode },
  { id: 5, name: 'Topic 5', icon: faBook },
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



.sidebar {
  width: 250px;
  /*background-color: #2c3e50;*/
  border-right: 1px solid blue;
  color: #000;
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
  color: #000;
  padding: 10px;
  cursor: pointer;
  border-radius: 5px;
  margin-bottom: 15px;
  transition: background-color 0.3s;
}

.sidebar li:hover {
  background-color: #34495e;
  color: #fff;
}

.sidebar li.active {
  background-color: #2980b9;
  color: #fff;
}

.icon {
  margin-right: 10px;
  font-size: 1.5em;
}
