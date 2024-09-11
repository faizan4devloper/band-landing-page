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
