import React, { useState, useEffect } from "react";
import Modal from "react-modal";
import { useNavigate } from "react-router-dom";
import styles from "./Header.module.css";
import lightLogo from "./HCLTechLogoBlue.svg"; // Blue logo for light theme
import darkLogo from "./HCLTechLogoWhite.png"; // White logo for dark theme
import RequestDemoForm from "./RequestDemoForm";
import FeedbackForm from "./FeedbackForm";
import feedbackImg from './feedback10.svg';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';
import { faMoon, faSun } from '@fortawesome/free-solid-svg-icons';

const Header = ({ theme, setTheme }) => {
  const [modalIsOpen, setModalIsOpen] = useState(false);
  const [feedbackModalIsOpen, setFeedbackModalIsOpen] = useState(false);
  const [isVisible, setIsVisible] = useState(true);
  const [lastScrollY, setLastScrollY] = useState(0);
  const navigate = useNavigate();

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
    <div className={`${styles.navbarWrapper} ${isVisible ? styles.show : styles.hide}`}>
      <nav className={styles.header}>
        <div className={styles.logo}>
          <img
            src={theme === 'light' ? lightLogo : darkLogo} // Conditional logo rendering based on theme
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



/* Default Light Theme */
:root {
  --primary-color: #5f1ec1;
  --secondary-color: rgba(15, 95, 220, 1);
  --background-color: #ffffff;
  --text-color: #808080;
  --toggle-icon-color: #000000;
  --scrollbar-color: rgba(15, 95, 220, 1);
  --scrollbar-background: #dcdcdc;
  --button-background-color: rgba(13, 85, 198, 0.1);
  --button-hover-color: #5f1ec1;
  --navbar-background: #fff;
    --toggle-circle: #000;

  --navbar-border: rgba(219, 197, 255, 1);
  --modal-background: #ffffff;
  --overlay-background: rgba(0, 0, 0, 0.15);
}

/* Dark Theme */
[data-theme="dark"] {
  --primary-color: #9d66f5;
  --secondary-color: #c1a1f2;
  --background-color: #1a1a2e;
  --text-color: #ffffff;
  --scrollbar-color: #5f1ec1;
  --scrollbar-background: #333333;
  --button-background-color: rgba(95, 30, 193, 0.8);
  --button-hover-color: #c1a1f2;
  --navbar-background: linear-gradient(to bottom, #1a1a2e, #16213e);
  --toggle-circle: #fff;
  --navbar-border: rgba(62, 62, 62);
  --modal-background: #333;
  --overlay-background: rgba(0, 0, 0, 0.9);
}

/* .navbarWrapper Styles */
.navbarWrapper {
  position: fixed;
  top: 0;
  left: 0;
  min-width: 100%;
  z-index: 1000;
  background: var(--navbar-background);
  border-bottom: 0.1px solid var(--navbar-border);
  padding: 8px 0px;
  transition: transform 0.5s ease-in-out;
}

.navbarWrapper.hide {
  transform: translateY(-100%);
}

.navbarWrapper.show {
  transform: translateY(0);
}

.navbarWrapper .header {
  display: flex;
  align-items: center;
  justify-content: space-around;
  margin: 5px -200px;
}

.logo img {
  max-height: 20px;
  cursor: pointer;
}

.right {
  display: flex;
  align-items: center;
}

.button {
  display: flex;
  align-items: center;
  background: linear-gradient(90deg, var(--primary-color) 0%, var(--secondary-color) 100%);
  color: white;
  border: none;
  padding: 8px 8px;
  margin: 0px 5px;
  font-size: 12px;
  border-radius: 5px;
  cursor: pointer;
  transition: .6s ease;
}

.button:hover {
  transform: translate(0, -5px);
}

.content {
  padding-top: 70px; /* Adjust the value to match the height of your header */
}

.logo img::after {
  content: attr(title);
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  background-color: #333;
  color: #fff;
  padding: 5px;
  border-radius: 4px;
  font-size: 12px;
  white-space: nowrap;
  opacity: 0;
  visibility: hidden;
  transition: opacity 0.3s, visibility 0.1s;
}

.logo img:hover::after {
  opacity: 1;
  visibility: visible;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.modal {
  position: absolute;
  top: 50%;
  left: 50%;
  right: auto;
  bottom: auto;
  transform: translate(-50%, -50%);
  background: var(--modal-background);
  border-radius: 8px;
  padding: 20px;
  margin-top: 30px;
  max-width: 900px;
  width: 90%;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  animation: fadeIn 0.3s ease-out;
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: var(--overlay-background);
  animation: fadeIn 0.3s ease-out;
}

.button img {
  width: 13px;
  height: 13px;
}

.whiteImage {
  width: 28px;
  margin-left: 6px;
  cursor: pointer;
  transition: .6s ease;
}

.whiteImage:hover {
  transform: translate(0, -5px);
}

.feedbackbutton {
  background: transparent;
}

/* Theme Toggle Switch */
.themeToggleButton {
  position: relative;
  margin-right: 6px;
  width: 45px;
  height: 21px;
  background-color: var(--button-background-color);
  border-radius: 30px;
  cursor: pointer;
  display: flex;
  align-items: center;
  padding: 2px;
  transition: background-color 0.3s ease;
}

.themeToggleButton:hover {
  background-color: var(--button-hover-color);
}

.toggleCircle {
  position: absolute;
  top: 2px;
  left: 2px;
  width: 20px;
  height: 20px;
  background-color: var(--toggle-circle);
  border-radius: 50%;
  transition: transform 0.3s ease;
}

.themeToggleButton.light .toggleCircle {
  transform: translateX(0);
}

.themeToggleButton.dark .toggleCircle {
  transform: translateX(25px); /* Moves the circle to the right */
}

.toggleIcon {
  position: absolute;
  font-size: 14px;
  color: var(--toggle-icon-color);
  transition: opacity 0.3s ease;
}

.sunIcon {
  left: 5px;
      color:yellow;

  opacity: 0;
}

.moonIcon {
  right: 5px;

  opacity: 1;
}

.themeToggleButton.light .sunIcon {
  opacity: 1; /* Show sun icon in light mode */
}

.themeToggleButton.light .moonIcon {
  opacity: 0; /* Hide moon icon in light mode */
}

.themeToggleButton.dark .sunIcon {
  opacity: 0; /* Hide sun icon in dark mode */
}

.themeToggleButton.dark .moonIcon {
  opacity: 1; /* Show moon icon in dark mode */
}



import React, { useState, useEffect } from "react";
import { Carousel } from "react-responsive-carousel";
import "react-responsive-carousel/lib/styles/carousel.min.css";
import styles from "./MainContent.module.css";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faTimes, faChevronLeft, faChevronRight } from "@fortawesome/free-solid-svg-icons";
import Video from "./Video";
import BeatLoader from "react-spinners/BeatLoader";

const MainContent = ({ activeTab, content }) => {
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
      };
    }
  }, [maximizedImage]);

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
            title="Click to Enlarge"
          >
            <img
              src={image}
              alt={`Thumbnail ${index + 1}`}
              className={styles.customThumb}
              loading="lazy"
            />
          </div>
        ))}
      </div>
      <Carousel
        showArrows={false}
        showIndicators={false}
        showThumbs={false}
        showStatus={false}
        selectedItem={currentSlide}
        onChange={(index) => setCurrentSlide(index)}
        className={styles.customCarousel}
      >
        {images.map((image, index) => (
          <div
            key={index}
            onClick={() => toggleMaximize(image)}
          >
            <img src={image} alt={`Slide ${index + 1}`} title="Click to Enlarge" loading="lazy" />
          </div>
        ))}
      </Carousel>
    </div>
  );

  const renderContent = (content) => {
    if (!content || content.length === 0) {
      return <BeatLoader color="#5931d4" size={8} />;
    }

    if (typeof content === "string") {
      return <div>{content}</div>;
    }

    return content.length > 1
      ? renderCarousel(content)
      : (
        <img
          src={content[0]}
          alt="Single Image"
          className={maximizedImage === content[0] ? styles.maximized : ""}
          onClick={() => toggleMaximize(content[0])}
          title="Click To Enlarge"
        />
      );
  };

  const renderImageOrCarousel = (images) => {
    if (!images || images.length === 0) {
      return (
        <div className={styles.imageLoadingContainer}>
          <p className={styles.imageLoadingCaption}>Processing, please wait</p>
          <BeatLoader color="#5931d4" size={8} />
        </div>
      );
    }

    return renderContent(images);
  };

  if (!content) {
    return (
      <div className={styles.mainContent}>Content not available</div>
    );
  }

  const contentMap = {
    description: (
      <div className={styles.description}>
        {renderImageOrCarousel(content.description)}
      </div>
    ),
    solutionFlow: (
      <div className={styles.solution}>
        {renderImageOrCarousel(content.solutionFlow)}
      </div>
    ),
    demo: (
      <div className={styles.demo}>
        <Video src={content.demo} />
      </div>
    ),
    techArchitecture: (
      <div className={styles.architecture}>
        {renderImageOrCarousel(content.techArchitecture)}
      </div>
    ),
    benefits: (
      <div className={styles.benefits}>
        {renderImageOrCarousel(content.benefits)}
      </div>
    ),
    adoption: (
      <div className={styles.adoption}>
        {renderImageOrCarousel(content.adoption)}
      </div>
    ),
  };

  return (
    <div className={`${styles.mainContent} ${maximizedImage ? styles.laserCursorEnabled : ""}`}>
      {contentMap[activeTab] || <div>Content not available</div>}
      {maximizedImage && (
        <div className={styles.overlay}>
          <FontAwesomeIcon
            icon={faTimes}
            className={styles.closeIcon}
            onClick={() => setMaximizedImage(null)}
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



/* Default Light Theme */
:root {
  --background-color: #ffffff;
  --background-gradient: linear-gradient(to bottom, #ffffff, #f0f0f0);
  --box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  --scrollbar-track: #f1f1f1;
  --scrollbar-thumb: #5f1ec1;
  --scrollbar-thumb-hover: #3d1299;
  --border-color: rgba(95, 30, 193, 0.8);
  --image-border: transparent;
  --section-background: #f9f9f9;
  --section-border: rgba(95, 30, 193, 0.8);
  --highlight-color: #5f1ec1;
  --laser-cursor-color: #ff0000;
  --laser-cursor-shadow: 4px 3px 20px rgba(255, 0, 0, 54.8), 0 0 11px rgba(255, 0, 0, 202.6);
  --nav-button-color: #ffffff;
  --nav-button-hover: #808080;
}

/* Dark Theme */
[data-theme="dark"] {
  --background-color: #1a1a2e;
  --background-gradient: linear-gradient(to bottom, #1a1a2e, #16213e);
  --box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
  --scrollbar-track: #333333;
  --scrollbar-thumb: #9d66f5;
  --scrollbar-thumb-hover: #c1a1f2;
  --border-color: rgba(95, 30, 193, 0.8);
  --image-border: transparent;
  --section-background: rgba(0, 0, 0, 0.1);
  --section-border: rgba(95, 30, 193, 0.8);
  --highlight-color: #9d66f5;
  --laser-cursor-color: #ff0000;
  --laser-cursor-shadow: 4px 3px 20px rgba(255, 0, 0, 54.8), 0 0 11px rgba(255, 0, 0, 202.6);
  --nav-button-color: #ffffff;
  --nav-button-hover: #808080;
}

/* Main content styling */
.mainContent {
  display: flex;
  flex-direction: column;
  width: 100%;
  padding: 0px 20px;
  background-color: var(--background-color);
  box-shadow: var(--box-shadow);
  height: calc(100vh - 100px);
  overflow-y: auto;
  min-height: 300px;
}

/* Custom Scrollbar Styling */
.mainContent::-webkit-scrollbar {
  width: 12px;
}

.mainContent::-webkit-scrollbar-track {
  background: var(--scrollbar-track);
}

.mainContent::-webkit-scrollbar-thumb {
  background-color: var(--scrollbar-thumb);
  border-radius: 20px;
  border: 3px solid var(--scrollbar-track);
}

.mainContent::-webkit-scrollbar-thumb:hover {
  background-color: var(--scrollbar-thumb-hover);
}

/* Image styling before maximization */
.mainContent img {
  max-width: 90%;
  height: auto;
  display: block;
  margin: 10px auto;
  transition: transform 0.3s ease;
  z-index: 1100;
  cursor: pointer;
}

/* Image styling after maximization */
.maximized,
.maximizedImage {
  max-width: 100%;
  max-height: 100%;
  width: 100%;
  height: 100%;
  object-fit: contain;
  margin: auto;
  display: block;
  transition: transform 0.3s ease;
  cursor: none; /* Hide default cursor after maximization */
  z-index: 1200;
}

/* Ensures the cursor is hidden over the maximized image */
.maximizedImage {
  width: 77%;
  height: 100%;
  object-fit: contain;
  cursor: none !important; /* Hide default cursor after maximization */
  z-index: 1200;
}

/* General cursor hiding for maximized images and overlays */
.maximized,
.maximizedImage,
.overlay {
  cursor: none;
}

/* Close icon styling */
.closeIcon {
  position: absolute;
  top: 10px;
  right: 125px;
  font-size: 18px;
  color: var(--nav-button-color);
  cursor: pointer;
  z-index: 1300;
}

.closeIcon:hover {
  color: var(--nav-button-hover);
}

/* Overlay styling */
.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.9);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1200;
}

/* Section styling */
.benefits,
.description,
.demo,
.architecture,
.adoption,
.solution {
  padding: 10px 15px;
  background-color: var(--section-background);
  border-left: 4px solid var(--section-border);
  margin-bottom: 20px;
  box-shadow: var(--box-shadow);
  margin-bottom: 50px;
}

.highlight {
  font-style: italic;
  color: var(--highlight-color);
  font-weight: bold;
}

/* Carousel and thumb styling */
.carouselContainer {
  display: flex;
}

.customThumbs {
  display: flex;
  flex-direction: column;
}

.customThumbContainer {
  cursor: pointer;
}

.customThumb {
  width: 100px;
  height: 100px;
  object-fit: cover;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border: 2px solid var(--image-border);
  transition: border-color 0.3s;
}

.customThumb:hover,
.selected .customThumb {
  border-color: var(--highlight-color);
}

.customCarousel {
  flex: 1;
  cursor: pointer;
}

/* Image loading container */
.imageLoadingContainer {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 20px;
  min-height: 340px;
  min-width: 610px;
}

.imageLoadingCaption {
  font-size: 12px;
  font-weight: bold;
  color: var(--highlight-color);
  margin-bottom: 10px;
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.1);
  }
  100% {
    transform: scale(1);
  }
}

/* Laser cursor enabled */
.laserCursorEnabled {
  display: block;
  cursor: none !important;
}

.laserCursor {
  position: fixed;
  width: 10px; /* Adjust size to resemble a laser pointer */
  height: 10px; /* Adjust size to resemble a laser pointer */
  background-color: var(--laser-cursor-color); /* Red color for the laser cursor */
  border-radius: 50%; /* Make it a circle */
  pointer-events: none;
  z-index: 1500;
  box-shadow: var(--laser-cursor-shadow);
  animation: pulseLaser 1.5s infinite;
}

.laserCursor::before {
  content: "";
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 4px; /* Adjust the size of the white center */
  height: 4px; /* Adjust the size of the white center */
  background-color: #ffffff; /* White color for the center */
  border-radius: 50%; /* Make it a circle */
}

/* Navigation button styling */
.navButton {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  font-size: 24px;
  color: var(--nav-button-color);
  cursor: pointer;
  z-index: 1300;
}

.navButton:hover {
  color: var(--nav-button-hover);
}

.navButton:first-child {
  left: 60px;
}

.navButton:last-child {
  right: 60px;
}
