import React, { useState, useEffect, useCallback } from 'react';
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

  // Memoize the fetchData function
  const fetchData = useCallback(async () => {
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
  }, [location.search]);

  useEffect(() => {
    fetchData();
  }, [fetchData]);

  useEffect(() => {
    if (setHeaderZIndex) { // Check if setHeaderZIndex is defined
      setHeaderZIndex(0); // Set zIndex to 0 for this page
      return () => setHeaderZIndex(1000); // Reset zIndex on unmount
    }
  }, [setHeaderZIndex]);

  // Memoize the handleBackButtonClick function
  const handleBackButtonClick = useCallback(() => {
    window.history.back();
  }, []);

  // Memoize the handleTabChange function
  const handleTabChange = useCallback((tabId) => {
    setActiveTab(tabId);
  }, []);

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
