import React, { useState } from 'react';
import { BrowserRouter as Router, Routes, Route, useLocation } from 'react-router-dom';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faChevronDown, faChevronUp } from '@fortawesome/free-solid-svg-icons';
import Header from './components/Header/Header';
import Home from './Home';
import SideBarPage from './components/Sidebar/SideBarPage';
import AllCardsPage from './components/Cards/AllCardsPage';
import Chatbot from './components/ChatBot/Chatbot';
import Footer from './components/Footer/Footer';
import { getCardsData } from './data';
import { BeatLoader } from 'react-spinners';
import styles from './App.module.css';

const MainApp = () => {
  const location = useLocation();
  const [cardsData, setCardsData] = useState([]);
  const [currentIndex, setCurrentIndex] = useState(0);
  const [bigIndex, setBigIndex] = useState(null);
  const [showScrollDown, setShowScrollDown] = useState(true);
  const [showScrollUp, setShowScrollUp] = useState(false);
  const [loading, setLoading] = useState(true);
  const [theme, setTheme] = useState(localStorage.getItem('theme') || 'light');
  const [isImageMaximized, setIsImageMaximized] = useState(false); // New state
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
    <div className={styles.app}>
      {loading ? (
        <div className={styles.loader}>
          <BeatLoader color="#5931d5" loading={loading} size={15} margin={2} />
        </div>
      ) : (
        <>
          <Header theme={theme} setTheme={setTheme} isImageMaximized={isImageMaximized} />
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
                  setIsImageMaximized={setIsImageMaximized} // Pass state setter
                />
              }
            />
            <Route
              path="/dashboard"
              element={<SideBarPage theme={theme} setTheme={setTheme} />}
            />
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
  );
};

const App = () => (
  <Router>
    <MainApp />
  </Router>
);

export default App;



import React, { useState, useEffect } from "react";
import { Carousel } from "react-responsive-carousel";
import "react-responsive-carousel/lib/styles/carousel.min.css";
import styles from "./MainContent.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faTimes, faChevronLeft, faChevronRight } from "@fortawesome/free-solid-svg-icons";
import Video from "./Video";
import BeatLoader from "react-spinners/BeatLoader";

const MainContent = ({ activeTab, content, setIsImageMaximized }) => {
  const [maximizedImage, setMaximizedImage] = useState(null);
  const [currentImageIndex, setCurrentImageIndex] = useState(0);
  const [laserPos, setLaserPos] = useState({ x: 0, y: 0 });
  const [currentSlide, setCurrentSlide] = useState(0);

  const allImages = [
    ...(content.description || []),
    ...(content.solutionFlow || []),
    ...(content.techArchitecture || []),
    ...(content.benefits || []),
    ...(content.adoption || []),
  ];

  const toggleMaximize = (imageSrc) => {
    const index = allImages.indexOf(imageSrc);
    if (index !== -1) {
      setCurrentImageIndex(index);
      setMaximizedImage(imageSrc);
      setIsImageMaximized(true); // Set state to true
    }
  };

  const closeMaximizedImage = () => {
    setMaximizedImage(null);
    setIsImageMaximized(false); // Set state to false
  };

  useEffect(() => {
    if (maximizedImage) {
      const handleMouseMove = (e) => {
        setLaserPos({ x: e.clientX, y: e.clientY });
      };
      const handleKeyDown = (e) => {
        if (e.key === "ArrowLeft") {
          handlePrevImage();
        } else if (e.key === "ArrowRight") {
          handleNextImage();
        }
      };

      document.addEventListener("mousemove", handleMouseMove);
      document.addEventListener("keydown", handleKeyDown);

      return () => {
        document.removeEventListener("mousemove", handleMouseMove);
        document.removeEventListener("keydown", handleKeyDown);
      };
    }
  }, [maximizedImage]);

  const handleNextImage = () => {
    if (allImages.length === 0) return;
    const isLastImage = currentImageIndex === allImages.length - 1;
    if (isLastImage) {
      closeMaximizedImage(); // Minimize if it's the last image
    } else {
      const nextIndex = (currentImageIndex + 1) % allImages.length;
      setCurrentImageIndex(nextIndex);
      setMaximizedImage(allImages[nextIndex]);
    }
  };

  const handlePrevImage = () => {
    if (allImages.length === 0) return;
    const isFirstImage = currentImageIndex === 0;
    if (isFirstImage) {
      closeMaximizedImage(); // Minimize if it's the first image
    } else {
      const prevIndex = (currentImageIndex - 1 + allImages.length) % allImages.length;
      setCurrentImageIndex(prevIndex);
      setMaximizedImage(allImages[prevIndex]);
    }
  };

  const renderCarousel = (images) => (
    <div className={styles.carouselContainer}>
      <div className={styles.carouselInner}>
        <Carousel
          selectedItem={currentSlide}
          onChange={(index) => setCurrentSlide(index)}
          showArrows={false}
          showStatus={false}
          showThumbs={false}
          autoPlay={false}
        >
          {images.map((image, index) => (
            <div key={index} onClick={() => toggleMaximize(image)}>
              <img src={image} alt={`slide ${index}`} className={styles.image} />
            </div>
          ))}
        </Carousel>
      </div>
    </div>
  );

  return (
    <div className={`${styles.mainContent} ${maximizedImage ? styles.laserCursorEnabled : ""}`}>
      {activeTab === "video" && <Video />}
      {content && renderCarousel(allImages)}

      {maximizedImage && (
        <div className={styles.overlay}>
          <FontAwesomeIcon
            icon={faTimes}
            className={styles.closeIcon}
            onClick={closeMaximizedImage}
          />
          <img
            src={maximizedImage}
            alt="Maximized view"
            className={styles.maximizedImage}
          />
          <div className={styles.navigationButtons}>
            <FontAwesomeIcon icon={faChevronLeft} onClick={handlePrevImage} className={styles.navButton} title="Prev"/>
            <FontAwesomeIcon icon={faChevronRight} onClick={handleNextImage} className={styles.navButton} title="Next"/>
          </div>
        </div>
      )}
      {maximizedImage && (
        <div
          className={styles.laserCursor}
          style={{ top: `${laserPos.y}px`, left: `${laserPos.x}px` }}
        />
      )}
    </div>
  );
};

export default MainContent;


import React from "react";
import styles from "./Header.module.css";

const Header = ({ theme, setTheme, isImageMaximized }) => {
  return (
    <div
      className={styles.navbarWrapper}
      style={{ zIndex: isImageMaximized ? 0 : 10 }} // Adjust z-index based on image maximization
    >
      <nav className={styles.header}>
        <button onClick={() => setTheme(theme === "light" ? "dark" : "light")}>
          Switch to {theme === "light" ? "Dark" : "Light"} Theme
        </button>
      </nav>
    </div>
  );
};

export default Header;
