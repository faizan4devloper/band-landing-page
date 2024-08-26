// HeaderContext.js
import React, { createContext, useState, useContext } from 'react';

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



// App.js
import React from 'react';
import { BrowserRouter as Router } from 'react-router-dom';
import { HeaderProvider } from './context/HeaderContext'; // Update path accordingly
import YourRoutes from './YourRoutes'; // Your routing component

function App() {
  return (
    <Router>
      <HeaderProvider>
        <YourRoutes />
      </HeaderProvider>
    </Router>
  );
}

export default App;



// Header.js
import React, { useEffect, useRef, useState } from 'react';
import Modal from 'react-modal';
import { useNavigate } from 'react-router-dom';
import styles from './Header.module.css';
import logoImage from './HCLTechLogo.svg';
import RequestDemoForm from './RequestDemoForm';
import laserStyles from './LaserCursor.module.css';
import { useHeaderContext } from '../context/HeaderContext'; // Update path accordingly

const Header = ({ zIndex }) => {
  const { headerZIndex, setHeaderZIndex } = useHeaderContext(); // Use context
  const [modalIsOpen, setModalIsOpen] = useState(false);
  const [isVisible, setIsVisible] = useState(true);
  const [isLaserEnabled, setIsLaserEnabled] = useState(false);
  const [cursorPosition, setCursorPosition] = useState({ x: 0, y: 0 });
  const [lastScrollY, setLastScrollY] = useState(0);
  const navigate = useNavigate();

  useEffect(() => {
    setHeaderZIndex(zIndex); // Update context with z-index
  }, [zIndex, setHeaderZIndex]);

  const handleImageClick = () => {
    navigate('/');
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
    return () => {
      window.removeEventListener('scroll', controlHeaderVisibility);
    };
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



// SideBarPage.js
import React, { useState, useEffect } from 'react';
import { useLocation } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowLeft } from '@fortawesome/free-solid-svg-icons';
import Header from '../Header/Header';
import SideBar from './SideBar';
import MainContent from './MainContent';
import styles from './SideBarPage.module.css';
import { getCardsData } from '../../data';

const SideBarPage = () => {
  const [activeTab, setActiveTab] = useState('description');
  const [cardContent, setCardContent] = useState({});
  const [cardTitle, setCardTitle] = useState('');
  const location = useLocation();

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

  const handleTabChange = (tabId) => {
    setActiveTab(tabId);
  };

  const handleBackButtonClick = () => {
    window.history.back();
  };

  return (
    <div className={styles.sideBarPage}>
      <Header zIndex={0} /> {/* Set zIndex to 0 */}
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

