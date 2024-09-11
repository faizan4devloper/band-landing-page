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
   
