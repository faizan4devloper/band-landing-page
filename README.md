/* Theme Toggle Switch */
.themeToggleButton {
  position: relative;
  width: 50px;
  height: 25px;
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
  background-color: #fff;
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
  color: var(--text-color);
  transition: opacity 0.3s ease;
}

.sunIcon {
  left: 5px;
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
import Modal from "react-modal";
import { useNavigate } from "react-router-dom";
import styles from "./Header.module.css";
import logoImage from "./HCLTechLogo.svg";
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
          <img
            src={feedbackImg}
            className={`${styles.feedbackImage} ${styles.whiteImage}`}
            alt="feedback"
            onClick={openFeedbackModal}
            title="Provide Feedback"
          />
          <div
            className={`${styles.themeToggleButton} ${theme === 'light' ? styles.light : styles.dark}`}
            onClick={handleThemeToggle}
            title="Toggle Theme"
          >
            <div className={styles.toggleCircle}></div>
            <FontAwesomeIcon icon={faSun} className={`${styles.toggleIcon} ${styles.sunIcon}`} />
            <FontAwesomeIcon icon={faMoon} className={`${styles.toggleIcon} ${styles.moonIcon}`} />
          </div>
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
