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
