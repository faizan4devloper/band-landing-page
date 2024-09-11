import React, { createContext, useContext, useState } from 'react';

const ZIndexContext = createContext();

export const ZIndexProvider = ({ children }) => {
  const [zIndexes, setZIndexes] = useState({
    header: 1,
    mainContent: 1,
  });

  const setZIndex = (component, zIndex) => {
    setZIndexes(prev => ({ ...prev, [component]: zIndex }));
  };

  return (
    <ZIndexContext.Provider value={{ zIndexes, setZIndex }}>
      {children}
    </ZIndexContext.Provider>
  );
};

export const useZIndex = () => useContext(ZIndexContext);




import React, { useState, useEffect } from "react";
import Modal from "react-modal";
import { useNavigate } from "react-router-dom";
import styles from "./Header.module.css";
import lightLogo from "./HCLTechLogoBlue.svg";
import darkLogo from "./HCLTechLogoWhite.png";
import RequestDemoForm from "./RequestDemoForm";
import FeedbackForm from "./FeedbackForm";
import feedbackImg from './feedback10.svg';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faMoon, faSun } from '@fortawesome/free-solid-svg-icons';
import { useZIndex } from './ZIndexContext'; // Import the context

const Header = ({ theme, setTheme }) => {
  const [modalIsOpen, setModalIsOpen] = useState(false);
  const [feedbackModalIsOpen, setFeedbackModalIsOpen] = useState(false);
  const [isVisible, setIsVisible] = useState(true);
  const [lastScrollY, setLastScrollY] = useState(0);
  const navigate = useNavigate();
  const { zIndexes, setZIndex } = useZIndex(); // Use context values

  useEffect(() => {
    // Set header z-index on mount
    setZIndex('header', zIndexes.header);

    // Clean up on unmount
    return () => {
      setZIndex('header', 1); // Reset z-index if needed
    };
  }, [setZIndex, zIndexes.header]);

  const handleImageClick = () => {
    navigate("/");
  };

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

  const handleThemeToggle = () => {
    if (typeof setTheme === 'function') {
      setTheme(theme === 'light' ? 'dark' : 'light');
    } else {
      console.error('setTheme is not a function');
    }
  };

  return (
    <div className={`${styles.navbarWrapper} ${isVisible ? styles.show : styles.hide}`} style={{ zIndex: zIndexes.header }}>
      <nav className={styles.header}>
        <div className={styles.logo}>
          <img
            src={theme === 'light' ? lightLogo : darkLogo}
            alt="HCLTech Logo"
            onClick={handleImageClick}
            title="Navigate to Home"
          />
        </div>

        <div className={styles.right}>
          <div
            className={`${styles.themeToggleButton} ${theme === 'light' ? styles.light : styles.dark}`}
            onClick={handleThemeToggle}
            title="Toggle Theme"
          >
            <div className={styles.toggleCircle}></div>
            <FontAwesomeIcon icon={faSun} className={`${styles.toggleIcon} ${styles.sunIcon}`} />
            <FontAwesomeIcon icon={faMoon} className={`${styles.toggleIcon} ${styles.moonIcon}`} />
          </div>
          <button className={styles.button} onClick={openModal}>
            Request For Live Demo
          </button>
          <img
            src={feedbackImg}
            className={`${styles.feedbackImage} ${styles.whiteImage}`}
            alt="feedback"
            onClick={openFeedbackModal}
            title="Provide Feedback"
          />

          <Modal isOpen={modalIsOpen} onRequestClose={closeModal} className={styles.modal}>
            <RequestDemoForm closeModal={closeModal} />
          </Modal>
          <Modal isOpen={feedbackModalIsOpen} onRequestClose={closeFeedbackModal} className={styles.modal}>
            <FeedbackForm closeModal={closeFeedbackModal} />
          </Modal>
        </div>
      </nav>
    </div>
  );
};

export default Header;



import React, { useState, useEffect } from "react";
import { Carousel } from "react-responsive-carousel";
import "react-responsive-carousel/lib/styles/carousel.min.css";
import styles from "./MainContent.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faTimes, faChevronLeft, faChevronRight } from "@fortawesome/free-solid-svg-icons";
import Video from "./Video";
import BeatLoader from "react-spinners/BeatLoader";
import { useZIndex } from './ZIndexContext'; // Import the context

const MainContent = ({ activeTab, content }) => {
  const [maximizedImage, setMaximizedImage] = useState(null);
  const [currentImageIndex, setCurrentImageIndex] = useState(0);
  const [laserPos, setLaserPos] = useState({ x: 0, y: 0 });
  const [currentSlide, setCurrentSlide] = useState(0);
  const { zIndexes, setZIndex } = useZIndex(); // Use context values

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
      setZIndex('mainContent', 10); // Set higher z-index
    }
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
        setZIndex('mainContent', 1); // Reset z-index when minimized
      };
    }
  }, [maximizedImage, setZIndex]);

  const handleNextImage = () => {
    if (allImages.length === 0) return;
    const isLastImage = currentImageIndex === allImages.length - 1;
    if (isLastImage) {
      setMaximizedImage(null); // Minimize if it's the last image
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
      setMaximizedImage(null); // Minimize if it's the first image
    } else {
      const prevIndex = (currentImageIndex - 1 + allImages.length) % allImages.length;
      setCurrentImageIndex(prevIndex);
      setMaximizedImage(allImages[prevIndex]);
    }
  };

  const renderCarousel = (images) => (
    <div className={styles.carouselContainer}>
      <div className={styles.customThumbs}>
        {images.map((image, index) => (
          <div
            key={index}
            className={`${styles.customThumbContainer} ${currentSlide === index ? styles.selected : ""}`}
            onClick={() => setCurrentSlide(index)}
            title="View image"
          >
            <img src={image} alt={`Thumbnail ${index}`} className={styles.customThumb} />
          </div>
        ))}
      </div>
      <Carousel
        selectedItem={currentSlide}
        onChange={(index) => setCurrentSlide(index)}
        showThumbs={false}
        infiniteLoop
        useKeyboardArrows
        dynamicHeight
        showArrows={false}
      >
        {images.map((image, index) => (
          <div key={index} onClick={() => toggleMaximize(image)}>
            <img src={image} alt={`Slide ${index}`} className={styles.carouselImage} />
          </div>
        ))}
      </Carousel>
    </div>
  );

  const renderContent = (contentArray) => {
    if (contentArray.length === 0) {
      return (
        <div className={styles.loaderContainer}>
          <BeatLoader color="#5931d4" size={8} />
        </div>
      );
    }

    if (maximizedImage) {
      return (
        <div
          className={styles.maximizedImageContainer}
          style={{ zIndex: zIndexes.mainContent }}
        >
          <img
            src={maximizedImage}
            alt="Maximized Image"
            className={styles.maximizedImage}
          />
          <button
            className={styles.arrowButton}
            onClick={handlePrevImage}
            title="Previous Image"
          >
            <FontAwesomeIcon icon={faChevronLeft} />
          </button>
          <button
            className={styles.arrowButton}
            onClick={handleNextImage}
            title="Next Image"
          >
            <FontAwesomeIcon icon={faChevronRight} />
          </button>
          <button
            className={styles.closeButton}
            onClick={() => setMaximizedImage(null)}
            title="Close"
          >
            <FontAwesomeIcon icon={faTimes} />
          </button>
        </div>
      );
    }

    return renderCarousel(allImages);
  };

  return (
    <div className={styles.mainContent} style={{ zIndex: zIndexes.mainContent }}>
      {activeTab === "description" && renderContent(content.description)}
      {activeTab === "solutionFlow" && renderContent(content.solutionFlow)}
      {activeTab === "techArchitecture" && renderContent(content.techArchitecture)}
      {activeTab === "benefits" && renderContent(content.benefits)}
      {activeTab === "adoption" && renderContent(content.adoption)}

      {maximizedImage && (
        <div
          className={styles.laserPointer}
          style={{
            left: `${laserPos.x}px`,
            top: `${laserPos.y}px`,
            zIndex: 10
          }}
        ></div>
      )}
    </div>
  );
};

export default MainContent;



import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import { ZIndexProvider } from './ZIndexContext'; // Import the provider

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <ZIndexProvider>
      <App />
    </ZIndexProvider>
  </React.StrictMode>
);
