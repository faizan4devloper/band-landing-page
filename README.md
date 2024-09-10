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
            <Header theme={theme} setTheme={setTheme} />
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



/* Define CSS variables for both light and dark themes */
:root {
  /* Light theme variables */
  --primary-color: #5f1ec1;
  --secondary-color: rgba(15, 95, 220, 1);
  --background-color: #ffffff;
  --text-color: #808080;
  --scrollbar-color: rgba(15, 95, 220, 1);
  --scrollbar-background: #dcdcdc;
  --button-background-color: rgba(13, 85, 198, 0.1);
  --button-hover-color: #5f1ec1;
}

[data-theme="dark"] {
  /* Dark theme variables */
  --primary-color: #9d66f5;
  --secondary-color: #c1a1f2;
  --background-color: #1a1a2e;
  --text-color: #ffffff;
  --scrollbar-color: #5f1ec1;
  --scrollbar-background: #333333;
  --button-background-color: rgba(95, 30, 193, 0.8);
  --button-hover-color: #c1a1f2;
}


::-webkit-scrollbar {
  width: 4px;
}


::-webkit-scrollbar-thumb {
  background: var(--scrollbar-color);
  border-radius: 10px;
}

html, body {
  font-family: "Poppins", sans-serif;
  background-color: var(--background-color);
  color: var(--text-color);/*background:rgba(28, 28, 1);*/


}

.app {
  width: 1100px;
  margin: 0 auto;
  
}

.cardsContainer {
  gap: 20px;
  margin-top: 80px;
  border-radius: 12px;
  display: flex;
  justify-content: center;
  flex-wrap: wrap; /* Allow wrapping */
  
}

.arrow {
  cursor: pointer;
  position: relative;
  top: 50%;
  transform: translateY(-50%);
  font-size: 18px;
  width: 18px;
  height: 18px;
  padding: 5px 5px 5px 5px;
  border-radius: 50px;
  border: 2px solid var(--secondary-color);
  color: rgba(15, 95, 220, 1);
  transition: transform 0.5s ease, background 0.5s ease;
}

.arrow:hover {
  background-color: var(--secondary-color);
  color: white;
}

.leftArrow {
  left: -25px;
  top: 90px;
}

.rightArrow {
  right: -25px;
  top: 90px;
}

@media screen and (max-width: 1100px) {
  .app {
    padding: 0 10px;
  }
}

@media screen and (max-width: 768px) {
  .app {
    max-width: 100%;
  }
}

.viewAllContainer {
  width: 100%;
  display: flex;
  justify-content: right;
  align-items: center;
  margin-right: 112px;
}

.solutionHead {
  font-weight: 600;
  font-size: 14px;
  color: #808080;
  margin-left: 118px;
}

.viewAllButton {
  font-weight: 600;
  font-size: 14px;
  color: var(--text-color);
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease, color 0.3s ease;
  text-decoration: none;
  display: inline-flex;
  align-items: center;
  padding: 6px 7px;
  border-radius: 5px;
    background-color: var(--button-background-color);
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
}

.viewAllButton:hover {
  transform: translateY(-5px);
  color: var(--button-hover-color);
  background-color: rgba(13, 85, 198, 0.1);
  box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.2);
}

.icon {
  margin-left: 8px;
  transition: transform 0.3s ease;
}

.viewAllButton:hover .icon {
  transform: translateX(5px);
}

.scrollDownButton,
.scrollUpButton {
  position: fixed;
  left: 20px;
  bottom: 20px;
  background: linear-gradient(90deg, var(--primary-color) 0%, var(--secondary-color) 100%);
  color: white;
  border: none;
  border-radius: 4px;
  width: 21px;
  height: 22px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 12px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease;
}

.scrollDownButton:hover, .scrollUpButton:hover {
  background-color: var(--button-hover-color);
}

/* Add this to your existing styles */
.loader {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh; /* Full height */
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  background-color: rgba(255, 255, 255, 0.9); /* Semi-transparent background */
  z-index: 1000; /* Make sure loader appears above other content */
}

.videoContainer {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 20px 0;
  transition: transform 0.6s ease-in-out, opacity 0.6s ease-in-out;
  opacity: 0;
  width: 100%;
  height: 95vh;
  transform: translateX(-100%); /* Start hidden to the left */
}

.videoContainer.big {
  transform: translateX(0); /* Slide in from left */
  opacity: 1;
}

.videoContainer.small {
  transform: translateX(-100%); /* Slide out to the left */
  opacity: 0; /* Smoothly disappear */
}

.video {
  width: 100%;
  height: 100%;
  object-fit: fill;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  cursor: pointer;
  transition: transform 0.5s ease-in-out, filter 0.5s ease-in-out;
}

.video:hover {
  transform: scale(1.02);
  filter: brightness(1.1);
}

.playPauseButton {
  position: absolute;
  bottom: 20px;
  right: 25px;
  background-color: var(--primary-color);
  color: white;
  border: none;
  padding: 12px 15px;
  cursor: pointer;
  transition: transform 0.3s ease-in-out, background-color 0.3s ease-in-out, box-shadow 0.3s ease-in-out;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
}

.playPauseButton:hover {
  background-color: var(--button-hover-color);
  transform: scale(1.2); /* Added rotation for a dynamic effect */
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.4);
}

@keyframes pulse {
  0% {
    transform: scale(1);
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  }
  50% {
    transform: scale(1.1);
    box-shadow: 0 0 20px rgba(95, 30, 193, 0.5);
  }
  100% {
    transform: scale(1);
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  }
}

.playPauseButton.pulse {
  animation: pulse 2s infinite;
}


/* Desktop and large screens */
@media screen and (min-width: 1024px) {
  .app {
    width: 1100px;
    margin: 0 auto;
  }

  .allCardsPage {
    padding: 20px;
    margin-top: 112px;
    margin-left: 270px;
    position: fixed;
  }

  .cardsContainer {
    gap: 20px;
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
  }
}

/* Tablet screens */
@media screen and (min-width: 768px) and (max-width: 1024px) {
  .app {
    width: 100%;
    padding: 0 20px;
  }

  .allCardsPage {
    margin-left: 220px;
  }

  .cardsContainer {
    grid-template-columns: repeat(3, 1fr);
    
  }

  .sidebar {
    width: 200px;
  }
}

/* code for ChatBot */

.volumeControl {
  position: absolute;
  bottom: 20px;
  right: 900px;
  display: flex;
  align-items: center;
}
 
.volumeIcon {
  color: white;
  font-size: 20px;
  cursor: pointer;
  margin-right: 10px;
}
 
.volumeSlider {
  -webkit-appearance: none;
  width: 100px;
  height: 5px;
  background: #ccc;
  border-radius: 5px;
  outline: none;
}
 
.volumeSlider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 15px;
  height: 15px;
  background: var(--primary-color);
  border-radius: 50%;
  cursor: pointer;
}
 
.volumeSlider::-moz-range-thumb {
  width: 15px;
  height: 15px;
  background: var(--primary-color);
  border-radius: 50%;
  cursor: pointer;
}


/* Theme Toggle Button */
.themeToggleButton {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background-color: var(--button-background-color);
  border: none;
  color: var(--text-color);
  padding: 10px 20px;
  border-radius: 30px;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 8px;
  transition: background-color 0.3s ease, transform 0.3s ease;
}

.themeToggleButton:hover {
  background-color: var(--button-hover-color);
  transform: scale(1.1);
}

.themeIcon {
  font-size: 16px;
}

.themeText {
  font-size: 14px;
  font-weight: 500;
}
