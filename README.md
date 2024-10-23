import React, { useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronRight } from '@fortawesome/free-solid-svg-icons';
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
      <h2 className={styles.sidebarHeads}>Suggested Topics</h2>
      <ul>
        {topics.map((topic) => (
          <li
            key={topic.id}
            className={`${styles.topic} ${active === topic.id ? styles.active : ''}`}
            onClick={() => handleClick(topic.id)}
          >
            <span className={styles.topicName}>{topic.name}</span>
            <FontAwesomeIcon icon={faChevronRight} className={styles.chevron} />
          </li>
        ))}
      </ul>
    </div>
  );
};

export default Sidebar;


.sidebar {
  width: 250px;
  border-right: 1px solid rgba(0, 0, 0, 0.1);
  color: #333;
  padding: 20px;
  height: 100vh;
  background: #f7f9fc; /* Light background for contrast */
  box-shadow: 2px 0 5px rgba(0, 0, 0, 0.1); /* Subtle shadow for depth */
}

.sidebarHeads {
  text-align: center;
  font-size: 1.5rem; /* Increased size for better visibility */
  color: #2e8b57; /* HCLTech theme color */
  margin-bottom: 20px; /* Spacing below header */
}

.sidebar ul {
  list-style-type: none;
  padding: 0;
}

.topic {
  display: flex;
  justify-content: space-between; /* Aligns text and icon */
  align-items: center;
  padding: 12px 20px; /* Adjust padding for a cleaner look */
  cursor: pointer;
  border-radius: 8px; /* More rounded corners for a modern feel */
  margin-bottom: 10px;
  transition: background-color 0.3s ease, transform 0.3s ease; /* Added transform transition */
}

.topic:hover {
  background: rgba(255, 255, 255, 0.3); /* HCLTech theme hover color */
  color: #2e8b57; /* HCLTech theme text color on hover */
  transform: translateY(-2px); /* Slight lift effect on hover */
}

.active {
  background: linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%);
  color: #fff; /* White text for active state */
}

.chevron {
  font-size: 1.2em; /* Chevron size */
  margin-left: auto; /* Push chevron to the right */
  transition: transform 0.3s; /* Add transition for animations */
}

.topic:hover .chevron {
  transform: translateX(5px); /* Move chevron on hover */
}

/* Optional: Add a transition for active topic change */
.topic.active {
  transition: background-color 0.3s ease;
}
