import React, { useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronRight } from '@fortawesome/free-solid-svg-icons'; // Import chevron icon
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
  background-color: rgba(44, 62, 80, 0.9); /* Semi-transparent background */
  border-right: 1px solid rgba(255, 255, 255, 0.1); /* Light border */
  color: #fff; /* White text color */
  padding: 20px;
  height: 100vh;
  box-shadow: 2px 0 10px rgba(0, 0, 0, 0.5); /* Subtle shadow for depth */
}

.sidebar ul {
  list-style-type: none;
  padding: 0;
}

.sidebar li {
  display: flex;
  justify-content: space-between; /* Space between text and icon */
  align-items: center;
  padding: 10px;
  cursor: pointer;
  border-radius: 5px;
  margin-bottom: 15px;
  transition: background-color 0.3s, color 0.3s; /* Transition for color change */
}

.sidebar li:hover {
  background-color: rgba(52, 73, 94, 0.8); /* Hover color with transparency */
  color: #fff; /* Ensure text remains white on hover */
}

.sidebar li.active {
  background-color: rgba(41, 128, 185, 0.9); /* Active topic color */
  color: #fff; /* White text for active topic */
}

.topicName {
  font-size: 1.1em; /* Slightly larger text for topic names */
}

.chevron {
  font-size: 1.2em; /* Adjust chevron size */
  transition: transform 0.3s; /* Transition for chevron icon */
}

.sidebar li:hover .chevron {
  transform: translateX(5px); /* Slight movement on hover */
}
