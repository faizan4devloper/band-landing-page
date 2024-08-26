// Header.js
import React, { useEffect, useRef } from "react";
import Modal from "react-modal";
import { useNavigate } from "react-router-dom";
import styles from "./Header.module.css";
import logoImage from "./HCLTechLogo.svg";
import RequestDemoForm from "./RequestDemoForm";
import laserStyles from "./LaserCursor.module.css";
import { useHeaderContext } from "./HeaderContext";

const Header = ({ zIndex }) => {
  const [modalIsOpen, setModalIsOpen] = useState(false);
  const [isVisible, setIsVisible] = useState(true);
  const [isLaserEnabled, setIsLaserEnabled] = useState(false);
  const [cursorPosition, setCursorPosition] = useState({ x: 0, y: 0 });

  const [lastScrollY, setLastScrollY] = useState(0);
  const navigate = useNavigate();
  const laserCursorRef = useRef(null);
  const { headerZIndex } = useHeaderContext(); // Get z-index from context

  useEffect(() => {
    document.documentElement.style.setProperty('--header-z-index', headerZIndex); // Set CSS variable
  }, [headerZIndex]);

  const handleImageClick = () => {
    navigate("/");
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
    window.addEventListener("scroll", controlHeaderVisibility);
    return () => {
      window.removeEventListener("scroll", controlHeaderVisibility);
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
      document.addEventListener("mousemove", handleMouseMove);
    } else {
      document.removeEventListener("mousemove", handleMouseMove);
    }
    return () => {
      document.removeEventListener("mousemove", handleMouseMove);
    };
  }, [isLaserEnabled]);

  return (
    <>
      <header
        className={`${styles.navbarWrapper} ${isVisible ? styles.show : styles.hide}`}
        style={{ zIndex: zIndex || headerZIndex }} // Apply z-index from prop or context
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
      <Header zIndex={0} /> {/* Set header zIndex to 0 */}
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


/* Header.module.css */
.navbarWrapper {
  position: fixed;
  top: 0;
  left: 0;
  min-width: 100%;
  background-color: #fff;
  border-bottom: 0.1px solid rgba(219, 197, 255, 1);
  padding: 8px 0px;
  z-index: var(--header-z-index, 1000); /* Default z-index */
  transition: transform 0.5s ease-in-out;
}

/* Additional styles */
