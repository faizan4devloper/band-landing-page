// src/context/HeaderContext.js
import React, { createContext, useContext, useState } from 'react';

const HeaderContext = createContext();

export const useHeaderContext = () => {
  return useContext(HeaderContext);
};

export const HeaderProvider = ({ children }) => {
  const [headerZIndex, setHeaderZIndex] = useState(1000); // Default zIndex

  return (
    <HeaderContext.Provider value={{ headerZIndex, setHeaderZIndex }}>
      {children}
    </HeaderContext.Provider>
  );
};


// src/App.js
import React from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import { HeaderProvider } from './context/HeaderContext'; // Import HeaderProvider
import Header from './components/Header/Header';
import Home from './Home';
import SideBarPage from './components/Sidebar/SideBarPage';
import AllCardsPage from './components/Cards/AllCardsPage';
import Chatbot from './components/ChatBot/Chatbot';
import Footer from './components/Footer/Footer';
import styles from './App.module.css';

const MainApp = () => {
  // Existing state and functions...

  return (
    <HeaderProvider> {/* Wrap your app with HeaderProvider */}
      <div className={styles.app}>
        {loading ? (
          <div className={styles.loader}>
            <BeatLoader color="#5931d5" loading={loading} size={15} margin={2} />
          </div>
        ) : (
          <>
            <Header />
            <Routes>
              <Route
                path="/"
                element={
                  <Home
                    cardsData={cardsData}
                    handleClickLeft={handleClickLeft}
                    handleClickRight={handleClickRight}
                    currentIndex={currentIndex}
                    bigIndex={bigIndex}
                    toggleSize={toggleSize}
                    cardsContainerRef={cardsContainerRef}
                  />
                }
              />
              <Route path="/dashboard" element={<SideBarPage />} />
              <Route
                path="/all-cards"
                element={<AllCardsPage cardsData={cardsData} cardsContainerRef={cardsContainerRef} />}
              />
            </Routes>
            {showScrollDown && location.pathname !== '/all-cards' && location.pathname !== '/dashboard' && (
              <div className={styles.scrollDownButton} onClick={handleScrollDown} title="Scroll Down">
                <FontAwesomeIcon icon={faChevronDown} />
              </div>
            )}
            {showScrollUp && (
              <div className={styles.scrollUpButton} onClick={handleScrollUp} title="Scroll Up">
                <FontAwesomeIcon icon={faChevronUp} />
              </div>
            )}
            <Chatbot />
            {showFooter && <Footer />}
          </>
        )}
      </div>
    </HeaderProvider>
  );
};

const App = () => (
  <Router>
    <MainApp />
  </Router>
);

export default App;





// src/components/Header/Header.js
import React, { useEffect, useState } from 'react';
import { useHeaderContext } from '../../context/HeaderContext'; // Import context
import Modal from 'react-modal';
import RequestDemoForm from './RequestDemoForm';
import styles from './Header.module.css';
import logoImage from './HCLTechLogo.svg';
import laserStyles from './LaserCursor.module.css';

const Header = () => {
  const { headerZIndex, setHeaderZIndex } = useHeaderContext(); // Use context
  const [modalIsOpen, setModalIsOpen] = useState(false);
  const [isVisible, setIsVisible] = useState(true);
  const [isLaserEnabled, setIsLaserEnabled] = useState(false);
  const [cursorPosition, setCursorPosition] = useState({ x: 0, y: 0 });
  const [lastScrollY, setLastScrollY] = useState(0);

  useEffect(() => {
    setHeaderZIndex(1000); // Set zIndex when component mounts
  }, [setHeaderZIndex]);

  const handleImageClick = () => {
    window.location.href = '/'; // Navigate to home
  };

  const openModal = () => {
    setModalIsOpen(true);
  };

  const closeModal = () => {
    setModalIsOpen(false);
  };

  const controlHeaderVisibility = () => {
    if (window.scrollY > lastScrollY) {
      setIsVisible(false);
    } else {
      setIsVisible(true);
    }
    setLastScrollY(window.scrollY);
  };

  useEffect(() => {
    window.addEventListener('scroll', controlHeaderVisibility);
    return () => window.removeEventListener('scroll', controlHeaderVisibility);
  }, [lastScrollY]);

  const handleMouseMove = (event) => {
    if (isLaserEnabled) {
      setCursorPosition({
        x: event.clientX,
        y: event.clientY,
      });
    }
  };

  useEffect(() => {
    if (isLaserEnabled) {
      document.addEventListener('mousemove', handleMouseMove);
    } else {
      document.removeEventListener('mousemove', handleMouseMove);
    }
    return () => {
      document.removeEventListener('mousemove', handleMouseMove);
    };
  }, [isLaserEnabled]);

  return (
    <>
      <header
        className={`${styles.navbarWrapper} ${isVisible ? styles.show : styles.hide}`}
        style={{ zIndex: headerZIndex }} // Use zIndex from context
      >
        <img
          src={logoImage}
          className={styles.logo}
          alt="HCL Technologies Logo"
          onClick={handleImageClick}
        />
        <button className={styles.demoButton} onClick={openModal}>
          Request a Demo
        </button>
      </header>

      <Modal isOpen={modalIsOpen} onRequestClose={closeModal} contentLabel="Request Demo Form">
        <RequestDemoForm />
      </Modal>

      {isLaserEnabled && (
        <div
          className={laserStyles.laserCursor}
          style={{ top: `${cursorPosition.y}px`, left: `${cursorPosition.x}px` }}
        />
      )}
    </>
  );
};

export default Header;


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
import { useHeaderContext } from '../../context/HeaderContext'; // Import context

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
    setHeaderZIndex(0); // Set zIndex to 0 for this page
    return () => setHeaderZIndex(1000); // Reset zIndex on unmount
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
