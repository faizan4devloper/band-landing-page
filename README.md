// Sidebar.js
import React, { useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronRight } from '@fortawesome/free-solid-svg-icons'; // Add chevron icon
import styles from './Sidebar.module.css';

const topics = [
  { id: 1, name: 'Topic 1' },
  { id: 2, name: 'Topic 2' },
  { id: 3, name: 'Topic 3' },
  { id: 4, name: 'Topic 4' },
  { id: 5, name: 'Topic 5' },
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
            <span>{topic.name}</span>
            <FontAwesomeIcon icon={faChevronRight} className={styles.chevron} />
          </li>
        ))}
      </ul>
    </div>
  );
};

export default Sidebar;


/* Sidebar.module.css */

.sidebar {
  width: 250px;
  border-right: 1px solid rgba(0, 0, 0, 0.1);
  color: #333;
  padding: 20px;
  height: 100vh;
  background-color: #f4f6f8; /* Light background for HCLTech theme */
}

.sidebar ul {
  list-style-type: none;
  padding: 0;
}

.sidebar li {
  display: flex;
  justify-content: space-between; /* Aligns text and icon */
  align-items: center;
  padding: 12px 15px; /* Adjust padding for a cleaner look */
  cursor: pointer;
  border-radius: 4px;
  margin-bottom: 10px;
  transition: background-color 0.3s ease;
}

.sidebar li:hover {
  background-color: rgba(46, 139, 87, 0.1); /* HCLTech theme hover color */
  color: #2e8b57; /* HCLTech theme text color on hover */
}

.sidebar li.active {
  background-color: rgba(46, 139, 87, 0.3); /* HCLTech theme active color */
  color: #fff; /* White text for active state */
}

.chevron {
  font-size: 1em; /* Chevron size */
  margin-left: auto; /* Push chevron to the right */
  transition: transform 0.3s; /* Add transition for animations */
}

.sidebar li:hover .chevron {
  transform: translateX(5px); /* Move chevron on hover */
}
