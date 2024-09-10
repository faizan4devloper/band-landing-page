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
    { id: "demo", label: "Demo Video", img: demoImg },
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


/* Default Light Theme */
:root {
  --sidebar-bg: #ffffff;
  --sidebar-border: rgba(219, 197, 255, 1);
  --menu-item-bg-hover: rgba(230, 235, 245, 1);
  --menu-item-active-bg: linear-gradient(90deg, #6f36cd 0%, #1f77f6 100%);
  --menu-item-active-color: white;
  --icon-img-filter: brightness(0) invert(0);
  --text-color: #000000; /* Default text color */
}

/* Dark Theme */
[data-theme="dark"] {
  --primary-color: #9d66f5;
  --secondary-color: #c1a1f2;
  --sidebar-bg: #2c2c2c;
  --sidebar-border: rgba(50, 50, 50, 1);
  --menu-item-bg-hover: rgba(70, 70, 70, 1); /* Adjusted hover color */
  --menu-item-active-bg: linear-gradient(90deg, #3a2d7f 0%, #2d5d8f 100%);
  --menu-item-active-color: #ffffff;
  --icon-img-filter: brightness(0) invert(1);
  --text-color: #ffffff; /* Dark theme text color */
}

/* Sidebar Styling */
.sideBar {
  width: 320px;
  min-width: 230px; /* Prevents shrinking */
  padding-left: 20px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  /*background-color: var(--sidebar-bg);*/
  border-right: 1px solid var(--sidebar-border);
  overflow-y: auto;
  overscroll-behavior: contain;
  scroll-behavior: smooth;
}

/* Menu Item Styling */
.menuItem {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  width: 100%; /* Ensure full width */
  min-width: 200px; /* Prevent shrinking */
  padding: 10px;
  margin: 5px 0;
  cursor: pointer;
  transition: background-color 0.3s;
  background: none;
  border: none;
  color: var(--text-color); /* Apply text color */
}

.menuItem:not(.active):hover {
  background-color: var(--menu-item-bg-hover);
  border-radius: 8px 0 0 8px;
}

.active {
  background: linear-gradient(90deg, var(--primary-color) 0%, var(--secondary-color) 100%);
  color: var(--menu-item-active-color);
  border-radius: 8px 0 0 8px;
}

.active .iconImg {
  filter: var(--icon-img-filter);
}

.iconImg {
  filter: var(--icon-img-filter);
}

/* Adjust any other necessary styles to match the dark theme */

.label {
  margin-right: auto;
  margin-left: 10px;
  font-size: 12px;
}





import React, { useState, useEffect } from 'react';
import { useLocation } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowLeft } from '@fortawesome/free-solid-svg-icons';
import Header from '../Header/Header';
import SideBar from './SideBar';
import MainContent from './MainContent';
import styles from './SideBarPage.module.css';
import { getCardsData } from '../../data';
import { useHeaderContext } from '../Context/HeaderContext'; // Import context

const SideBarPage = ({ theme, setTheme }) => {
  const [activeTab, setActiveTab] = useState('description');
  const [cardContent, setCardContent] = useState({});
  const [cardTitle, setCardTitle] = useState('');
  const location = useLocation();
  const { setHeaderZIndex } = useHeaderContext(); // Use context

  useEffect(() => {
    const fetchData = async () => {
      const data = await getCardsData();
      const params = new URLSearchParams(location.search);
      const title = params.get('title');
      if (title) {
        setCardTitle(title);
        const card = data.find((c) => c.title === title);
        if (card) {
          setCardContent(card.content);
        }
      }
    };

    fetchData();
  }, [location.search]);

  useEffect(() => {
    if (setHeaderZIndex) { // Check if setHeaderZIndex is defined
      setHeaderZIndex(0); // Set zIndex to 0 for this page
      return () => setHeaderZIndex(1000); // Reset zIndex on unmount
    }
  }, [setHeaderZIndex]);

  const handleTabChange = (tabId) => {
    setActiveTab(tabId);
  };

  const handleBackButtonClick = () => {
    window.history.back();
  };

  return (
    <div className={styles.sideBarPage}>
      <Header theme={theme} setTheme={setTheme} /> {/* Pass theme and setTheme */}
      <div className={styles.header2}>
        <button onClick={handleBackButtonClick} className={styles.backButton}>
          <FontAwesomeIcon icon={faArrowLeft} />
        </button>
        {cardTitle && <div className={styles.cardTitle}>{cardTitle}</div>}
      </div>
      <div className={styles.contentWrapper}>
        <SideBar activeTab={activeTab} handleTabChange={handleTabChange} />
        <MainContent activeTab={activeTab} content={cardContent} />
      </div>
    </div>
  );
};

export default SideBarPage;


.sideBarPage {
  /*display: flex;*/
  position: fixed;
  margin-top: 70px;
  margin-left: 45px;
  /* margin-top: 25px; */
  flex-direction: column;
  min-height: 100vh;
  overflow-y: auto; /* Enable vertical scrolling */
  overscroll-behavior: contain; /* Prevent overscrolling */
  scroll-behavior: smooth; /* Enable smooth scrolling */
  
}

.header2 {
  display: flex;
  align-items: center;
}
.cardTitle {
  font-size: 18px;
  color: rgba(23, 23, 25, 1);
  font-weight: 600;
  margin-bottom: 10px;
  font-family: "Poppins", sans-serif;
}
.contentWrapper {
  display: flex;
  /*margin-top: 15px;*/
  flex: 1;
}
.backButtonContainer {
  display: flex;
  align-items: center;
  padding: 10px;
}

.backButton {
  background-color: rgba(230, 235, 245, 1);
  padding: 7px;
  margin-left: 20px;
  margin-bottom: 10px;
  border-radius: 4px;
  width: 32px;
  font-size: 14px;
  border: none;
  cursor: pointer;
  margin-right: 10px;
}

.backButton:hover {
  color: rgba(95, 30, 193, 1); /* Change button color on hover */
}
