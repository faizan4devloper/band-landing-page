import React, { useState } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronRight } from '@fortawesome/free-solid-svg-icons';
import styles from './Sidebar.module.css';

// Import your images
import schoolingImg from '../../assets/schooling.png'; // Replace with actual path
import curriculumImg from '../../assets/curriculum.png'; // Replace with actual path
import supportImg from '../../assets/support.png'; // Replace with actual path

// Update topics to include images for each topic
const topics = [
  { id: 1, name: 'Schooling', image: schoolingImg },
  { id: 2, name: 'Curriculum and Programs', image: curriculumImg },
  { id: 3, name: 'Student and Community Support', image: supportImg },
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
            <img src={topic.image} alt={topic.name} className={styles.imgIcon} /> {/* Left side image */}
            <span className={styles.topicName}>{topic.name}</span> {/* Center text */}
            <FontAwesomeIcon icon={faChevronRight} className={styles.chevron} /> {/* Right chevron */}
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
}

.sidebar ul {
  list-style-type: none;
  padding: 0;
}

.sidebarHeads {
  text-align: center;
  font-size: 1rem;
}

.sidebar li {
  display: flex;
  align-items: center; /* Align items vertically in the center */
  padding: 12px 20px; /* Adjust padding for a cleaner look */
  cursor: pointer;
  border-radius: 4px;
  margin-bottom: 10px;
  font-size: 0.8rem;
  transition: background-color 0.3s ease;
}

.sidebar li:hover {
  background: rgba(230, 235, 245, 1); /* HCLTech theme hover color */
}

.sidebar li.active {
  background: linear-gradient(90deg, rgb(95, 30, 193) 0%, rgb(15, 95, 220) 100%) 0% 0% repeat rgba(0, 0, 0, 0);
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

.imgIcon {
  width: 20px; /* Adjust icon size as needed */
  margin-right: 10px; /* Space between image and text */
}
