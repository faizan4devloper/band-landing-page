// src/components/Header/Header.js
import React, { useEffect, useState } from 'react';
import { useHeaderContext } from '../../context/HeaderContext'; // Import context
import Modal from 'react-modal';
import RequestDemoForm from './RequestDemoForm';
import styles from './Header.module.css';
import logoImage from './HCLTechLogo.svg';
import laserStyles from './LaserCursor.module.css';

const Header = () => {
  const { headerZIndex, setHeaderZIndex } = useHeaderContext(); // Use context
  const [modalIsOpen, setModalIsOpen] = useState(false);
  const [isVisible, setIsVisible] = useState(true);
  const [isLaserEnabled, setIsLaserEnabled] = useState(false);
  const [cursorPosition, setCursorPosition] = useState({ x: 0, y: 0 });
  const [lastScrollY, setLastScrollY] = useState(0);

  useEffect(() => {
    setHeaderZIndex(1000); // Set zIndex when component mounts
  }, [setHeaderZIndex]);

  const handleImageClick = () => {
    window.location.href = '/'; // Navigate to home
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
    window.addEventListener('scroll', controlHeaderVisibility);
    return () => window.removeEventListener('scroll', controlHeaderVisibility);
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
      document.addEventListener('mousemove', handleMouseMove);
    } else {
      document.removeEventListener('mousemove', handleMouseMove);
    }
    return () => {
      document.removeEventListener('mousemove', handleMouseMove);
    };
  }, [isLaserEnabled]);

  return (
    <>
      <header
        className={`${styles.navbarWrapper} ${isVisible ? styles.show : styles.hide}`}
        style={{ zIndex: headerZIndex }} // Use zIndex from context
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
