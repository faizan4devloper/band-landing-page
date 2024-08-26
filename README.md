import React, { useState } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faAngleRight, faBars, faTimes } from "@fortawesome/free-solid-svg-icons";
import styles from "./SideBar.module.css";

// Import individual images for each menu item
import descriptionImg from "./Icons/Description.svg";
import solutionFlowImg from "./Icons/SolutionFlow.svg";
import demoImg from "./Icons/Demo.svg";
import techArchitectureImg from "./Icons/ArchitectureFlow.svg";
import benefitsImg from "./Icons/Benefits.svg";
import industyImg from "./Icons/Industry.svg";

const SideBar = ({ activeTab, handleTabChange, children }) => {
  const [isVisible, setIsVisible] = useState(true);

  const menuItems = [
    { id: "description", label: "Description", img: descriptionImg },
    { id: "solutionFlow", label: "Solution Flow", img: solutionFlowImg },
    { id: "demo", label: "Demo", img: demoImg },
    {
      id: "techArchitecture",
      label: "Technical Architecture",
      img: techArchitectureImg,
    },
    { id: "benefits", label: "Benefits", img: benefitsImg },
    { id: "adoption", label: "Industry Adoption", img: industyImg },
  ];

  const toggleSidebar = () => {
    setIsVisible(!isVisible);
  };

  return (
    <div className={styles.wrapper}>
      <button className={styles.toggleButton} onClick={toggleSidebar}>
        <FontAwesomeIcon icon={isVisible ? faTimes : faBars} />
      </button>
      <nav className={`${styles.sideBar} ${isVisible ? styles.visible : styles.hidden}`}>
        {menuItems.map((item) => (
          <button
            key={item.id}
            onClick={() => handleTabChange(item.id)}
            className={`${styles.menuItem} ${activeTab === item.id ? styles.active : ""}`}
          >
            <img
              src={item.img}
              className={`${styles.iconImg} ${styles.hoverEffect}`}
            />
            <span className={styles.label}>{item.label}</span>
            <FontAwesomeIcon icon={faAngleRight} className={styles.icon} />
          </button>
        ))}
      </nav>
      <div className={styles.content}>
        {children}
      </div>
    </div>
  );
};

export default SideBar;


.wrapper {
  display: flex;
  transition: margin-left 0.3s ease;
}

.sideBar {
  width: 340px;
  padding-left: 20px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  border-right: 1px solid rgba(219, 197, 255, 1);
  overflow-y: auto; /* Enable vertical scrolling */
  overscroll-behavior: contain; /* Prevent overscrolling */
  scroll-behavior: smooth; /* Enable smooth scrolling */
  transition: transform 0.3s ease;
}

.hidden {
  transform: translateX(-100%); /* Hide sidebar */
}

.visible {
  transform: translateX(0); /* Show sidebar */
}

.toggleButton {
  position: absolute;
  top: 10px;
  right: -40px; /* Position the button outside the sidebar */
  background: #6f36cd;
  color: white;
  border: none;
  padding: 10px;
  border-radius: 50%;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.toggleButton:hover {
  background: #1f77f6;
}

.content {
  flex: 1;
  padding: 20px;
  transition: margin-left 0.3s ease;
}

.hidden ~ .content {
  margin-left: 0; /* Adjust content margin when sidebar is hidden */
}

.visible ~ .content {
  margin-left: 340px; /* Adjust content margin when sidebar is visible */
}

.menuItem {
  display: flex;
  flex-direction: row; /* Make the menu item a row layout */
  justify-content: space-between; /* Align items with space between */
  align-items: center;
  width: 100%;
  padding: 10px;
  margin: 5px 0px;
  cursor: pointer;
  transition: background-color 0.3s; /* Add transition for smoother effect */
  background: none;
  border: none;
}

.menuItem:not(.active):hover {
  background-color: rgba(230, 235, 245, 1);
  border-radius: 8px 0 0 8px;
}

.active {
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  color: white;
  border-radius: 8px 0 0 8px; /* Add border radius to the left side */
}

.active .iconImg {
  filter: brightness(0) invert(1); /* Active color - white */
}

.iconImg {
  filter: brightness(0) invert(0); /* Default color - black */
}

.label {
  margin-right: auto; /* Push label text to the right */
  margin-left: 10px; /* Add some space between icon and label */
  font-size: 12px;
}











import React, { useState } from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faAngleRight, faBars, faTimes } from "@fortawesome/free-solid-svg-icons";
import styles from "./SideBar.module.css";

// Import individual images for each menu item
import descriptionImg from "./Icons/Description.svg";
import solutionFlowImg from "./Icons/SolutionFlow.svg";
import demoImg from "./Icons/Demo.svg";
import techArchitectureImg from "./Icons/ArchitectureFlow.svg";
import benefitsImg from "./Icons/Benefits.svg";
import industyImg from "./Icons/Industry.svg";

const SideBar = ({ activeTab, handleTabChange }) => {
  const [isVisible, setIsVisible] = useState(true);

  const menuItems = [
    { id: "description", label: "Description", img: descriptionImg },
    { id: "solutionFlow", label: "Solution Flow", img: solutionFlowImg },
    { id: "demo", label: "Demo", img: demoImg },
    {
      id: "techArchitecture",
      label: "Technical Architecture",
      img: techArchitectureImg,
    },
    { id: "benefits", label: "Benefits", img: benefitsImg },
    { id: "adoption", label: "Industry Adoption", img: industyImg },
  ];

  const toggleSidebar = () => {
    setIsVisible(!isVisible);
  };

  return (
    <div className={`${styles.container} ${isVisible ? styles.visible : styles.hidden}`}>
      <button className={styles.toggleButton} onClick={toggleSidebar}>
        <FontAwesomeIcon icon={isVisible ? faTimes : faBars} />
      </button>
      <nav className={styles.sideBar}>
        {menuItems.map((item) => (
          <button
            key={item.id}
            onClick={() => handleTabChange(item.id)}
            className={`${styles.menuItem} ${activeTab === item.id ? styles.active : ""}`}
          >
            <img
              src={item.img}
              className={`${styles.iconImg} ${styles.hoverEffect}`}
            />
            <span className={styles.label}>{item.label}</span>
            <FontAwesomeIcon icon={faAngleRight} className={styles.icon} />
          </button>
        ))}
      </nav>
    </div>
  );
};

export default SideBar;




.container {
  position: relative;
}

.sideBar {
  width: 340px;
  padding-left: 20px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  border-right: 1px solid rgba(219, 197, 255, 1);
  overflow-y: auto; /* Enable vertical scrolling */
  overscroll-behavior: contain; /* Prevent overscrolling */
  scroll-behavior: smooth; /* Enable smooth scrolling */
  transition: transform 0.3s ease;
}

.hidden {
  transform: translateX(-100%); /* Hide sidebar */
}

.visible {
  transform: translateX(0); /* Show sidebar */
}

.toggleButton {
  position: absolute;
  top: 10px;
  right: -40px; /* Position the button outside the sidebar */
  background: #6f36cd;
  color: white;
  border: none;
  padding: 10px;
  border-radius: 50%;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.toggleButton:hover {
  background: #1f77f6;
}

.menuItem {
  display: flex;
  flex-direction: row; /* Make the menu item a row layout */
  justify-content: space-between; /* Align items with space between */
  align-items: center;
  width: 100%;
  padding: 10px;
  margin: 5px 0px;
  cursor: pointer;
  transition: background-color 0.3s; /* Add transition for smoother effect */
  background: none;
  border: none;
}

.menuItem:not(.active):hover {
  background-color: rgba(230, 235, 245, 1);
  border-radius: 8px 0 0 8px;
}

.active {
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  color: white;
  border-radius: 8px 0 0 8px; /* Add border radius to the left side */
}

.active .iconImg {
  filter: brightness(0) invert(1); /* Active color - white */
}

.iconImg {
  filter: brightness(0) invert(0); /* Default color - black */
}

.label {
  margin-right: auto; /* Push label text to the right */
  margin-left: 10px; /* Add some space between icon and label */
  font-size: 12px;
}










/* eslint-disable jsx-a11y/alt-text */
import React from "react";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faAngleRight } from "@fortawesome/free-solid-svg-icons";
import styles from "./SideBar.module.css";

// Import individual images for each menu item
import descriptionImg from "./Icons/Description.svg";
import solutionFlowImg from "./Icons/SolutionFlow.svg";
import demoImg from "./Icons/Demo.svg";
import techArchitectureImg from "./Icons/ArchitectureFlow.svg";
import benefitsImg from "./Icons/Benefits.svg";
import industyImg from "./Icons/Industry.svg"

const SideBar = ({ activeTab, handleTabChange }) => {
  const menuItems = [
    { id: "description", label: "Description", img: descriptionImg },
    { id: "solutionFlow", label: "Solution Flow", img: solutionFlowImg },
    { id: "demo", label: "Demo", img: demoImg },
    {
      id: "techArchitecture",
      label: "Technical Architecture",
      img: techArchitectureImg,
    },
    { id: "benefits", label: "Benefits", img: benefitsImg },
    { id:"adoption", label: "Industry Adoption", img: industyImg  },
  ];

  return (
    <nav className={styles.sideBar}>
      {menuItems.map((item) => (
        <button
          key={item.id}
          onClick={() => handleTabChange(item.id)}
          className={`${styles.menuItem} ${
            activeTab === item.id ? styles.active : ""
          }`}
        >
          <img
            src={item.img}
            className={`${styles.iconImg} ${styles.hoverEffect}`}
          />
          <span className={styles.label}>{item.label}</span>
          <FontAwesomeIcon icon={faAngleRight} className={styles.icon} />
        </button>
      ))}
    </nav>
  );
};

export default SideBar;


/* SideBar.module.css */

.sideBar {
  width: 340px;
  padding-left: 20px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  border-right: 1px solid rgba(219, 197, 255, 1);
  overflow-y: auto; /* Enable vertical scrolling */
  overscroll-behavior: contain; /* Prevent overscrolling */
  scroll-behavior: smooth; /* Enable smooth scrolling */
}

.menuItem {
  display: flex;
  flex-direction: row; /* Make the menu item a row layout */
  justify-content: space-between; /* Align items with space between */
  align-items: center;
  width: 100%;
  padding: 10px;
  margin: 5px 0px;
  cursor: pointer;
  transition: background-color 0.3s; /* Add transition for smoother effect */
  background: none;
  border: none;
}

.menuItem:not(.active):hover {
  background-color: rgba(230, 235, 245, 1);
  border-radius: 8px 0 0 8px;
}

.active {
  background: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  color: white;
  border-radius: 8px 0 0 8px; /* Add border radius to the left side */
}

.active .iconImg {
  filter: brightness(0) invert(1); /* Active color - white */
}

.iconImg {
  filter: brightness(0) invert(0); /* Default color - black */
}

.label {
  margin-right: auto; /* Push label text to the right */
  margin-left: 10px; /* Add some space between icon and label */
  font-size: 12px;
}




