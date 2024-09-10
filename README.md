import React, { useState, useEffect, useRef } from 'react';
import { BrowserRouter as Router, Routes, Route, useLocation } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp, faMoon, faSun } from '@fortawesome/free-solid-svg-icons';
import Header from './components/Header/Header';
import Home from './Home';
import SideBarPage from './components/Sidebar/SideBarPage';
import AllCardsPage from './components/Cards/AllCardsPage';
import Chatbot from './components/ChatBot/Chatbot';
import Footer from './components/Footer/Footer';
import { getCardsData } from './data';
import { BeatLoader } from 'react-spinners';
import { HeaderProvider } from './components/Context/HeaderContext';
import styles from './App.module.css';

const MainApp = () => {
  const location = useLocation();
  const [cardsData, setCardsData] = useState([]);
  const [currentIndex, setCurrentIndex] = useState(0);
  const [bigIndex, setBigIndex] = useState(null);
  const [showScrollDown, setShowScrollDown] = useState(true);
  const [showScrollUp, setShowScrollUp] = useState(false);
  const [loading, setLoading] = useState(true);
  const [theme, setTheme] = useState(localStorage.getItem('theme') || 'light'); // State for theme
  const cardsContainerRef = useRef(null);

  useEffect(() => {
    const fetchData = async () => {
      const data = await getCardsData();
      setCardsData(data);
      setLoading(false);
    };

    fetchData();
  }, []);

  useEffect(() => {
    document.documentElement.setAttribute('data-theme', theme);
    localStorage.setItem('theme', theme);
  }, [theme]);

  const toggleSize = (index) => {
    setBigIndex(index === bigIndex ? null : index);
  };

  const handleClickLeft = () => {
    const newBigIndex = bigIndex === null || bigIndex === 0 ? cardsData.length - 1 : bigIndex - 1;
    setBigIndex(newBigIndex);
    const newCurrentIndex = newBigIndex < currentIndex ? newBigIndex : currentIndex;
    setCurrentIndex(newCurrentIndex);
  };

  const handleClickRight = () => {
    const newBigIndex = bigIndex === null || bigIndex === cardsData.length - 1 ? 0 : bigIndex + 1;
    setBigIndex(newBigIndex);
    const newCurrentIndex = newBigIndex > currentIndex + 4 ? newBigIndex - 4 : currentIndex;
    setCurrentIndex(newCurrentIndex);
  };

  const handleScrollDown = () => {
    if (cardsContainerRef.current) {
      cardsContainerRef.current.scrollIntoView({ behavior: 'smooth' });
      setShowScrollDown(false);
      setShowScrollUp(true);
    }
  };

  const handleScrollUp = () => {
    window.scrollTo({ top: 0, behavior: 'smooth' });
    setShowScrollDown(true);
    setShowScrollUp(false);
  };

  useEffect(() => {
    const handleScroll = () => {
      if (window.scrollY > 50) {
        setShowScrollUp(true);
        setShowScrollDown(false);
      } else {
        setShowScrollUp(false);
        setShowScrollDown(true);
      }
    };

    const debounceScroll = debounce(handleScroll, 100);
    window.addEventListener('scroll', debounceScroll);

    return () => window.removeEventListener('scroll', debounceScroll);
  }, []);

  const debounce = (func, wait) => {
    let timeout;
    return function executedFunction(...args) {
      const later = () => {
        clearTimeout(timeout);
        func(...args);
      };
      clearTimeout(timeout);
      timeout = setTimeout(later, wait);
    };
  };

  const showFooter = location.pathname === '/';

  return (
    <HeaderProvider>
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
            <button className={styles.themeToggleButton} onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
              <FontAwesomeIcon icon={theme === 'light' ? faMoon : faSun}
              className={styles.themeIcon}
              />
              <span className={styles.themeText}>
                {theme === 'light' ? 'Dark' : 'Light'} Mode
              </span>
            </button>
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




import React, { useState, useEffect } from "react";
import Modal from "react-modal";
import { useNavigate } from "react-router-dom";
import styles from "./Header.module.css";
import { useHeaderContext } from '../Context/HeaderContext'; // Import context
import logoImage from "./HCLTechLogo.svg";
import RequestDemoForm from "./RequestDemoForm";
import FeedbackForm from "./FeedbackForm"; // Import FeedbackForm

import feedbackImg from './feedback10.svg';

const Header = () => {
  const { headerZIndex, setHeaderZIndex } = useHeaderContext(); // Use context
  const [modalIsOpen, setModalIsOpen] = useState(false);
  const [feedbackModalIsOpen, setFeedbackModalIsOpen] = useState(false); // Feedback modal state
  const [isVisible, setIsVisible] = useState(true);
  const [lastScrollY, setLastScrollY] = useState(0);
  const navigate = useNavigate();

  const handleImageClick = () => {
    navigate("/");
  };
  
  useEffect(() => {
    setHeaderZIndex(1000); // Set zIndex when component mounts
  }, [setHeaderZIndex]);


  const openModal = () => {
    setModalIsOpen(true);
  };

  const closeModal = () => {
    setModalIsOpen(false);
  };

  const openFeedbackModal = () => {
    setFeedbackModalIsOpen(true);
  };

  const closeFeedbackModal = () => {
    setFeedbackModalIsOpen(false);
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

  return (
    <div className={`${styles.navbarWrapper} ${isVisible ? styles.show : styles.hide}`} style={{ zIndex: headerZIndex }}>
      <nav className={styles.header}>
        <div className={styles.logo}>
          <img
            src={logoImage}
            alt=""
            onClick={handleImageClick}
            title="Navigate to Home"
          />
        </div>
        <div className={styles.right}>
          <button className={styles.button} onClick={openModal}>
            Request For Live Demo
          </button>
  <img src={feedbackImg} className={`${styles.feedbackImage} ${styles.whiteImage}`} alt="feedback" onClick={openFeedbackModal} title="Provide Feedback"/>          
        </div>
      </nav>
      <div className={styles.border}></div>

      <Modal
        isOpen={modalIsOpen}
        onRequestClose={closeModal}
        contentLabel="Request for Live Demo!"
        className={styles.modal}
        overlayClassName={styles.overlay}
      >
        <RequestDemoForm closeModal={closeModal} />
      </Modal>

      <Modal
        isOpen={feedbackModalIsOpen}
        onRequestClose={closeFeedbackModal}
        contentLabel="Submit Feedback"
        className={styles.modal}
        overlayClassName={styles.overlay}
      >
        <FeedbackForm closeModal={closeFeedbackModal} />
      </Modal>
    </div>
  );
};

export default Header;
