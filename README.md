
// src/components/Sidebar/SideBarPage.js
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

const SideBarPage = () => {
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
      <Header />
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
