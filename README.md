
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
