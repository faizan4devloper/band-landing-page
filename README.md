import React, { useEffect } from 'react';
import { BrowserRouter as Router } from 'react-router-dom';
import MainApp from './MainApp';

const App = () => {
  useEffect(() => {
    const theme = localStorage.getItem('theme') || 'light';
    document.body.classList.toggle('dark-theme', theme === 'dark');
  }, []);

  return (
    <Router>
      <MainApp />
    </Router>
  );
};

export default App;


import React, { useRef, useState, useEffect } from 'react';
import { Routes, Route, Link, useLocation } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faArrowRight, faArrowLeft, faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';
import Header from './components/Header/Header';
import MyCarousel from './components/Carousel/MyCarousel';
import Cards from './components/Cards/Cards';
import styles from './App.module.css';
import SideBarPage from './components/Sidebar/SideBarPage';
import AllCardsPage from './components/Cards/AllCardsPage';
import { cardsData as initialCardsData } from './data';
import { BeatLoader } from 'react-spinners';

const Home = ({
  cardsData,
  handleClickLeft,
  handleClickRight,
  currentIndex,
  bigIndex,
  toggleSize,
  cardsContainerRef,
  handleMouseEnter,
}) => {
  const visibleCards = cardsData.slice(currentIndex, currentIndex + 5);
  return (
    <>
      <MyCarousel />
      <div
        className={styles.cardsContainer}
        ref={cardsContainerRef}
        onMouseEnter={handleMouseEnter}
      >
        <div className={styles.viewAllContainer}>
          <Link to="/all-cards" className={styles.viewAllButton}>
            View All Solutions <FontAwesomeIcon icon={faArrowRight} className={styles.icon} />
          </Link>
        </div>
        <span className={`${styles.arrow} ${styles.leftArrow}`} onClick={handleClickLeft}>
          <FontAwesomeIcon icon={faArrowLeft} title="Previous" />
        </span>
        {visibleCards.map((card, index) => {
          const actualIndex = currentIndex + index;
          return (
            <Cards
              key={index}
              imageUrl={card.imageUrl}
              title={card.title}
              description={card.description}
              isBig={actualIndex === bigIndex}
              toggleSize={() => toggleSize(actualIndex)}
            />
          );
        })}
        <span className={`${styles.arrow} ${styles.rightArrow}`} onClick={handleClickRight}>
          <FontAwesomeIcon icon={faArrowRight} title="Next" />
        </span>
      </div>
    </>
  );
};

const MainApp = () => {
  const location = useLocation();
  const [cardsData, setCardsData] = useState(initialCardsData);
  const [currentIndex, setCurrentIndex] = useState(0);
  const [bigIndex, setBigIndex] = useState(0);
  const [showScrollDown, setShowScrollDown] = useState(true);
  const [showScrollUp, setShowScrollUp] = useState(false);
  const [loading, setLoading] = useState(true);
  const cardsContainerRef = useRef(null);

  const toggleSize = (index) => {
    setBigIndex(index === bigIndex ? null : index);
  };

  const handleClickLeft = () => {
    const newBigIndex = bigIndex === 0 ? cardsData.length - 1 : bigIndex - 1;
    setBigIndex(newBigIndex);

    const newCurrentIndex = newBigIndex < currentIndex ? newBigIndex : currentIndex;
    setCurrentIndex(newCurrentIndex);
  };

  const handleClickRight = () => {
    const newBigIndex = bigIndex === cardsData.length - 1 ? 0 : bigIndex + 1;
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

  const handleMouseEnter = () => {
    if (cardsContainerRef.current) {
      cardsContainerRef.current.scrollIntoView({ behavior: 'smooth', block: 'start' });
    }
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

  useEffect(() => {
    const timer = setTimeout(() => {
      setLoading(false);
    }, 2000);

    return () => clearTimeout(timer);
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

  return (
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
                  handleMouseEnter={handleMouseEnter}
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
        </>
      )}
    </div>
  );
};

export default MainApp;

import React, { useState } from 'react';
import Modal from 'react-modal';
import { useNavigate } from 'react-router-dom';
import styles from './Header.module.css';
import logoImage from './HCL Tech.svg';
import RequestDemoForm from './RequestDemoForm';
import ThemeSwitcher from '../ThemeSwitcher'; // Import ThemeSwitcher

const Header = () => {
  const [modalIsOpen, setModalIsOpen] = useState(false);
  const navigate = useNavigate();

  const handleImageClick = () => {
    navigate('/');
  };

  const openModal = () => {
    setModalIsOpen(true);
  };

  const closeModal = () => {
    setModalIsOpen(false);
  };

  return (
    <div className={styles.navbarWrapper}>
      <nav className={styles.header}>
        <div className={styles.logo}>
          <img src={logoImage} alt="" onClick={handleImageClick} style={{ cursor: 'pointer' }} title="Navigate to Home"/>
        </div>
        <div className={styles.right}>
          <ThemeSwitcher /> {/* Add the theme switcher */}
          <button className={styles.button} onClick={openModal}>Request For live Demo</button>
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
    </div>
  );
};

export default Header;


:root {
  --background-color: #ffffff;
  --text-color: #000000;
  --primary-color: #5931d5;
  --secondary-color: rgba(15, 95, 220, 1);
  --hover-color: rgba(13, 85, 198, 0.1);
}

.dark-theme {
  --background-color: #121212;
  --text-color: #ffffff;
  --primary-color: #bb86fc;
  --secondary-color: #03dac6;
  --hover-color: #3700b3;
}

body {
  background-color: var(--background-color);
  color: var(--text-color);
}

a {
  color: var(--primary-color);
}

button {
  background: var(--primary-color);
}

button:hover {
  background: var(--hover-color);
}

.app {
  position: relative;
}

.loader {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.cardsContainer {
  display: flex;
  overflow-x: scroll;
  position: relative;
}

.arrow {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  cursor: pointer;
  font-size: 24px;
  color: var(--text-color);
}

.leftArrow {
  left: 10px;
}

.rightArrow {
  right: 10px;
}

.viewAllContainer {
  text-align: center;
}

.viewAllButton {
  background: none;
  border: none;
  font-size: 16px;
  color: var(--primary-color);
  cursor: pointer;
}

.icon {
  margin-left: 8px;
}

.scrollDownButton,
.scrollUpButton {
  position: fixed;
  bottom: 10px;
  right: 10px;
  cursor: pointer;
  font-size: 24px;
  color: var(--primary-color);
}

.scrollUpButton {
  bottom: 60px;
}

.scrollDownButton {
  right: 10px;
}

.border {
  height: 1px;
  background: var(--primary-color);
}

.modal {
  background-color: var(--background-color);
  color: var(--text-color);
}

.overlay {
  background: rgba(0, 0, 0, 0.75);
}


import React, { useState, useEffect } from 'react';
import styles from './ThemeSwitcher.module.css';

const ThemeSwitcher = () => {
  const [isDarkMode, setIsDarkMode] = useState(false);

  useEffect(() => {
    const theme = localStorage.getItem('theme') || 'light';
    setIsDarkMode(theme === 'dark');
    document.body.classList.toggle('dark-theme', theme === 'dark');
  }, []);

  const toggleTheme = () => {
    const newTheme = isDarkMode ? 'light' : 'dark';
    setIsDarkMode(!isDarkMode);
    document.body.classList.toggle('dark-theme', !isDarkMode);
    localStorage.setItem('theme', newTheme);
  };

  return (
    <button onClick={toggleTheme} className={styles.themeSwitcher}>
      {isDarkMode ? '🌞 Light Mode' : '🌙 Dark Mode'}
    </button>
  );
};

export default ThemeSwitcher;

.themeSwitcher {
  background: none;
  border: none;
  cursor: pointer;
  font-size: 16px;
  color: var(--text-color);
  transition: color 0.3s ease;
}

.themeSwitcher:hover {
  color: var(--primary-color);
}
